---
title: "AZ-104 Notes"
# get-date -format yyyy-MM-ddTHH:mm:ss
date: 2022-07-29T17:17:16
draft: false
author: "Golgothus"
description: "Microsoft AZ-104 Cloud Challenge Notes"
summary: "Host a web application in the Azure App Service"
tags: ["Azure", "Web Apps", Azure Portal]
featuredImage: "/images/city.jpg"
categories: ["Azure"]
---

# Prepare the web application code

## Bootstrap a web application (Python)

To create a new Python web app, we'll be using Flask, a commonly used web application framework.

```bash
pip install flask
```

After this is installed, we can then create a baseline environment for our Flask app:

```python
from flask import Flask
app = Flask(__name__)

@app.route("/")
def hello():
    return "Hello World!\n"
```

This will make every request to your app return the message "Hello World!"

## Adding your code to source control

Now it's time to add the current files to your repo.

```git
git init
git add .
git commit -m "Initial commit"
```

This is part of the CI/CD (continuous integration / continuous deployment) workflow. We'll be using a local git repo to host these files.

> Using CI/CD enables more frequent code deployment in a reliable manner by automating builds, tests, and deployments for every code change. It allows delivering new features and bug fixes for your application faster and more effectively.
> Note from Microsoft - https://docs.microsoft.com/en-us/learn/modules/host-a-web-app-with-azure-app-service/4-implement-a-web-app?pivots=python

# Exercise - Write code to implement a web application

## Create a new web project

From previous experience, I've heard it's important to use virtual environments when using Python. Virtual environments help to separate your dependencies and libraries, helping to keep your projects organized.

```bash
python3 -m venv venv
source venv/bin/activate
pip install flask
```

The code for the web app we'll be using in Flask:

```python3
from flask import Flask
app = Flask(__name__)

@app.route("/")
def hello():
    return "<html><body><h1>Hello Best Bike App!</h1></body></html>\n"
```

We can save a list of requirements for our app by performing the following:

```bash
pip freeze > requirements.txt
```

This command will [freeze](https://pip.pypa.io/en/stable/cli/pip_freeze/) all the packages in our virtual environment to a requirements file.

## Optionally test your app

Open a secondary Azure shell at https://shell.azure.com

From your first shell, cd into your app / working directory.

```bash
cd ~/app
export FLASK_APP=applciation.py
flask run
```

From our second shell, we can then use curl to request the webpage from our web app.

```bash
curl localhost:5000
```

We should then see the HTML returned from our webapp.

```html
<html><body><h1>Hello Best Bike App!</h1></body></html>
```

## Deploy with az webapp up

First we started with creating some variables containing the necessary values for the `az webapp up` command.

```bash
export APPNAME=$(az webapp list --query [0].name --output tsv)
export APPRG=$(az webapp list --query [0].resourceGroup --output tsv)
export APPPLAN=$(az appservice plan list --query [0].name --output tsv)
export APPSKU=$(az appservice plan list --query [0].sku.name --output tsv)
export APPLOCATION=$(az appservice plan list --query [0].location --output tsv)
```

Change directories to your app / working directory.

```bash
cd ~/app
```

Run the following to attribute our variables to their required fields.

```bash
az webapp up --name $APPNAME --resource-group $APPRG --plan $APPPLAN --sku $APPSKU --location "$APPLOCATION"
```

References:
- https://docs.microsoft.com/en-us/learn/modules/host-a-web-app-with-azure-app-service/?ns-enrollment-type=learningpath&ns-enrollment-id=learn.az-104-manage-compute-resources
- https://docs.microsoft.com/en-us/cli/azure/get-started-with-azure-cli
