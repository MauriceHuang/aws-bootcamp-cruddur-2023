# Week 0 — Billing and Architecture

## Homework Challenges
- [x] 1. ~~Destroy~~ Set up your root account credentials, Set MFA, IAM role
- [ ] 2. Use EventBridge to hookup Health Dashboard to SNS and send notification when there is a service health issue.
- [ ] 3. Review all the questions of each pillars in the Well Architected Tool (No specialized lens)
- [ ] 4. Create an architectural diagram (to the best of your ability) the CI/CD logical pipeline in Lucid Charts
- [ ] 5. Research the technical and service limits of specific services and how they could impact the technical path for technical flexibility. 
- [ ] 6. Open a support ticket and request a service limit
- [ ] 7. AWS CLI

### 7. AWS CLI

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

```shell
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```
