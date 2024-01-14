# go to dockerhub and load another dockerfile
FROM python:3.10-slim-buster 

#when the container starts, this is where the directory works from. e.g. your default directory when you first login into your laptop.
#This refers to the another OS inside the container
#makes a new folder in side conatainer
WORKDIR /backend-flask

# Outside Container -> Inside Conainer
# This contains the libraries required to run the app
COPY requirements.txt requirements.txt

# Install the python library inside the container  
RUN pip3 install -r requirements.txt

# .means everything in the curernt directory
# first period . /backend-flask (outside container)
# second period . /backend-flask (inside container)
COPY . .

# setting environment variable
# Inside container and will trmain set when the container is running
ENV FLASK_ENV=development

EXPOSE ${PORT}

# -m refers to run flask module 
CMD [ "python3", "-m" , "flask", "run", "--host=0.0.0.0", "--port=4567"]