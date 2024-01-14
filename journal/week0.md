# Week 0 — Billing and Architecture

## Homework Challenges
- [x] 1. ~~Destroy~~ Set up your root account credentials, Set MFA, IAM role
- [ ] 2. Use EventBridge to hookup Health Dashboard to SNS and send notification when there is a service health issue.
- [ ] 3. Review all the questions of each pillars in the Well Architected Tool (No specialized lens)
- [ ] 4. Create an architectural diagram (to the best of your ability) the CI/CD logical pipeline in Lucid Charts
- [ ] 5. Research the technical and service limits of specific services and how they could impact the technical path for technical flexibility. 
- [ ] 6. Open a support ticket and request a service limit
- [x] 7. AWS CLI
```bash ```
### 7. AWS CLI

Go to AWS, IAM>user> your username >  genrate access keys and secret access keys, set up gitpod environment variables.
[Activating](https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-parameters-prompting.html#cli-usage-auto-prompt-configure) auto-prompt on AWS couldshell
```shell
aws --cli-auto-prompt
```
Check credential
```shell
aws sts get-caller-identity
```

in gitpod, install awscli,
1. open terminal
2. navigate to /workspace 

To install the AWS CLI, run the following commands.

```shell
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```
It should return the following:
```
You can now run: /usr/local/bin/aws --version
```
You can also run the following script to run aws 
```
/usr/local/bin/aws
```
There's a standard linux directory structure. [Here](https://linuxhandbook.com/linux-directory-structure/).  

After installing program, they are ususally in `/usr/local/bin`

To update your current installation of the AWS CLI, add your existing symlink and installer information to construct the install command using the `--bin-dir`, `--install-dir`, and `--update` parameters. The following command block uses an example symlink of /usr/local/bin and example installer location of /usr/local/aws-cli.
```shell
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install --bin-dir /usr/local/bin --install-dir /usr/local/aws-cli --update
```
### How's typing `aws` it will automatically knows where to run? 
When you type `aws`` into a command line interface (CLI), the system does not automatically know where to run it from. Instead, it follows a specific process to locate the aws command. Here's how it typically works:

Search in Environment Variable's `Path`: The system looks through the directories listed in the `Path` environment variable. The `Path` variable contains a list of directories that the system searches for executable files.

Finding the Executable: If there's an executable file named `aws` (or `aws.exe` on Windows) in any of these directories, the system will execute it.

If Not Found: If the system doesn't find the aws executable in any of the directories listed in the `Path`, it will return an error, typically something like "command not found".

This is a standard procedure for most operating systems like Windows, macOS, and Linux when you enter a command in the terminal or command prompt. It allows the system to efficiently locate and execute the right program without the user having to specify the exact location of the executable file each time.
```bash
$ echo $PATH
```
It will return the following:
```bash
/workspace/.cargo/bin:/home/gitpod/.pyenv/shims:/workspace/go/bin:/home/gitpod/.nix-profile/bin:/ide/bin/remote-cli:/home/gitpod/.nix-profile/bin:/home/gitpod/.local/bin:/home/gitpod/.sdkman/candidates/maven/current/bin:/home/gitpod/.sdkman/candidates/java/current/bin:/home/gitpod/.sdkman/candidates/gradle/current/bin:/workspace/.cargo/bin:/home/gitpod/.rvm/gems/ruby-3.2.2/bin:/home/gitpod/.rvm/gems/ruby-3.2.2@global/bin:/home/gitpod/.rvm/rubies/ruby-3.2.2/bin:/workspace/go/bin:/home/gitpod/.nix-profile/bin:/ide/bin/remote-cli:/home/gitpod/go/bin:/home/gitpod/go-packages/bin:/home/gitpod/.nvm/versions/node/v20.10.0/bin:/home/gitpod/.yarn/bin:/home/gitpod/.pnpm:/home/gitpod/.pyenv/bin:/home/gitpod/.rvm/bin:/home/gitpod/.cargo/bin:/home/linuxbrew/.linuxbrew/bin:/home/linuxbrew/.linuxbrew/sbin/:/home/gitpod/.local/bin:/usr/games:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/home/gitpod/.nvm/versions/node/v20.10.0/bin:/home/gitpod/.rvm/bin:/home/gitpod/.nvm/versions/node/v20.10.0/bin:/home/gitpod/.rvm/bin
```
`$` refers to environment variable.
On terminal, type the following:
```
$ env
```
This will list down all the environment variable.

To insert environment varible for aws. Type the following in gitpod terminal, in `/workspace` directory. update the respective variables and enter into the terminal.

Here's an example:

```bash
gp env AWS_ACCESS_KEY_ID="AKIAIOSFODNN7EXAMPLE"
gp env AWS_SECRET_ACCESS_KEY="wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY"
gp env AWS_DEFAULT_REGION="us-west-2"
```
### Using AWS CLI to create budget and budget alarm.

```bash
aws budgets create-budget \
    --account-id $AWS_ACCOUNT_ID\
    --budget file://aws/json/budget.json \
    --notifications-with-subscribers file://aws/json/budget-notifications-with-subscribers.json
```
Getting `account-d` by entering the following `export AWS_ACCOUNT_ID=$(aws sts get-caller-identity --query Account --output text)` in the terminal


### Using AWS CLI to create a billing alarm

https://docs.aws.amazon.com/cli/latest/reference/sns/create-topic.html

```bash 
aws sns create-topic --billing-alarm
```
returns the following:
```bash
arn:aws:sns:ap-southeast-1:474584145453:billing-alarm
```
update the arn below and enter into terminal.
```bash 
aws sns subscribe \
    --topic-arn="arn:aws:sns:ap-southeast-1:474584145453:billing-alarm" \
    --protocol=email \
    --notification-endpoint=maurice.huang.pai.so@gmail.com
```

### Using AWS CLI to create a cloudwatch alarm

https://repost.aws/knowledge-center/cloudwatch-estimatedcharges-alarm
create a alarm-config.json
with the following script
```json
{
    "AlarmName": "DailyEstimatedCharges",
    "AlarmDescription": "This alarm would be triggered if the daily estimated charges exceeds 1$",
    "ActionsEnabled": true,
    "AlarmActions": [
        "arn:aws:sns:ap-southeast-1:474584145453:billing-alarm"
    ],
    "EvaluationPeriods": 1,
    "DatapointsToAlarm": 1,
    "Threshold": 1,
    "ComparisonOperator": "GreaterThanOrEqualToThreshold",
    "TreatMissingData": "breaching",
    "Metrics": [{
        "Id": "m1",
        "MetricStat": {
            "Metric": {
                "Namespace": "AWS/Billing",
                "MetricName": "EstimatedCharges",
                "Dimensions": [{
                    "Name": "Currency",
                    "Value": "USD"
                }]
            },
            "Period": 86400,
            "Stat": "Maximum"
        },
        "ReturnData": false
    },
    {
        "Id": "e1",
        "Expression": "IF(RATE(m1)>0,RATE(m1)*86400,0)",
        "Label": "DailyEstimatedCharges",
        "ReturnData": true
    }]
  }
```
Then run the follow code in terminal. 
```bash
aws cloudwatch put-metric-alarm --cli-input-json file://aws/json/alarm-config.json
```
