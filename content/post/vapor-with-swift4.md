+++
date = 2016-04-20
lastmod = 2017-12-23
draft = false
tags = ["Swift", "Vapor"]
title = "Vapor SetUp with Swift 4"
math = true
summary = """
Vapor is a Web framework with Swift as the programming language.
"""

[header]
#image = "headers/getting-started.png"
caption = "Image credit: [**Academic**](https://github.com/gcushen/hugo-academic/)"

+++

### Step by step guid to install Vapor2.0 and Postgres with Xcode 9.x and Swift 4:


- [Requirements](#requirements)
- [Vapor Project](#vapor-project)
- [Heroku Deployment](#heroku-deployment)
- [Postgres User and DB Creation](#postgres-user-and-db-creation)
- [Vapor Configuration](#vapor-configuration)



## Requirements

## Brew
Homebrew needs to be installed first to seamlessly install Vapor and Postgres. To install brew in Mac look at [Homebrew](https://brew.sh/)

##Postgresql
Once brew is installed install Postgresql

```bash
$ brew install postgres
```
Upon successful installation run the below command
```bash
$ brew tap homebrew/services
```
then start the Postgresql server by the following command
```bash
$ brew services start postgresql
```

## Vapor Project
Run the below command to check the compatibility with Vapor
```bash
$ curl -sL toolbox.qutheory.io | bash
```
based on the output of the above command install vapor. A general installation command is shown below
```bash
$ brew install vapor
$ brew install vapor/tap/vapor
$ vapor --help
```
The vapor --help should give you the options if the vapor installation is successful

To create a vapor project type
```bash
$ vapor new ProjectName
$ cd ProjectName
$ vapor update
```
when the vapor update is done it will ask for creating a xcode project type 'y' and press enter/return.
Once the Xcode project is created it will ask whether you want to open it? Type 'y' and press enter/return.

Now try to build the project by pressing cmd+B. If the build is successful you are lucky else close
the Xcode and go to terminal and delete the Package.resolved in the ProjectName folder with the command
```bash
$ rm Package.resolved
```
now enter the command
```bash
$ vapor update
```
Give 'y' for all the questions and press enter/return.

## Heroku Deployment
Install heroku using brew
```bash
$ brew install heroku
```
Login to Heroku
```bash
$ heroku Login
```
Now go to the ProjectName folder in terminal and run the below command and once the heroku
build is ready, answer the questions with the answers given below:
```bash
$ vapor heroku init
Would you like to provide a custom Heroku app name?
y/n> n
Would you like to deploy to a region other than the US?
y/n> n
https://<App-Name>.herokuapp.com/ | https://git.heroku.com/<App-Name>.git

Would you like to provide a custom Heroku buildpack?
y/n> n
Setting buildpack...
Are you using a custom Executable name?
y/n> n
Setting procfile...
Committing procfile...
Would you like to push to Heroku now?
y/n> y
```
## Postgres User and DB Creation

Creating a user with password:
```bash
CREATE ROLE <user_name> WITH LOGIN PASSWORD '<password>';
ALTER ROLE <user_name> CREATEDB;
CREATE DATABASE hello;
```
## Vapor Configuration

In Package.swift add the following line in dependeies:
```bash
dependencies: [
        .package(url: "https://github.com/vapor/vapor.git", .upToNextMajor(from: "2.1.0")),
        .package(url: "https://github.com/vapor/fluent-provider.git", .upToNextMajor(from: "1.2.0")),
        .package(url: "https://github.com/vapor-community/postgresql-provider.git", .upToNextMajor(from: "2.1.0")),
    ],
```

and ensure that the targets has the appropriate dependencies as shown below
```bash
targets: [
        .target(name: "App", dependencies: ["Vapor", "FluentProvider", "PostgreSQLProvider"],

```
Add a Procfile in the root level where the Xcode-Project file exists and add the below contents
```bash
$ vim Procfile
```
Add the below line inside the Procfile
```bash
web: Run --env=production --port=$PORT --config:postgresql.url=$DATABASE_URL
```

Create a directory named secrets inside the Config directory and add the postgresql.json file as shown below
```bash
#From the root
$ mkdir Config/secrets
$ vim Config/secrets/postgresql.json
    { "url" : "psql://user:pass@url/db" }
```

Close the Xcode project and run
```bash
vapor update
vapor xcode
```

Open the project and check whether the PostgreSQLProvider is added under the dependencies folder in Xcode.
If it is available then the installation is successful
