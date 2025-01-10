# Ideasoft DevOps Take-Home Assessment

## Table of Contents

- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Project Setup](#project-setup)
  - [1. Clone the Repository](#1-clone-the-repository)
  - [2. Environment Variables](#2-environment-variables)
  - [3. Docker Compose Setup](#3-docker-compose-setup)
- [Jenkins Pipeline](#jenkins-pipeline)
  - [Pipeline Stages](#pipeline-stages)
- [Usage](#usage)
- [Notes](#notes)
- [External Resources](#external-resources)
- [Contact](#contact)

## Introduction

This repository contains the setup and configuration for a CI/CD pipeline tailored for a PHP blog project. The pipeline utilizes Jenkins for automation, SonarQube for code quality analysis, and Docker for containerization. The primary goal is to automate the process of cloning the project, installing dependencies, running unit tests, performing code quality checks, and preparing the application for deployment.

## Prerequisites

Before setting up the project, ensure you have the following installed on your machine:

- [Docker](https://www.docker.com/get-started) (version 20.10 or later)
- [Docker Compose](https://docs.docker.com/compose/install/) (version 1.29 or later)
- [Git](https://git-scm.com/downloads)
- [Jenkins](https://www.jenkins.io/download/) (optional if using the provided Docker setup)

## Project Setup

### 1. Clone the Repository

First, clone this repository to your local machine:

```bash
git clone 
cd symphonydemo
```

### 2. Environment Variables
Create a .env file in the root directory to define necessary environment variables. This file will be used by Docker Compose to configure services.


```env
# SonarQube Database Credentials
POSTGRES_USER=sonar
POSTGRES_PASSWORD=sonar
SONAR_JDBC_URL=jdbc:postgresql://db:5432/sonar
SONAR_JDBC_USERNAME=${POSTGRES_USER}
SONAR_JDBC_PASSWORD=${POSTGRES_PASSWORD}

# Jenkins Credentials
JENKINS_TOKEN=your-jenkins-token
```

Note: Replace your-jenkins-token with your actual Jenkins token. Ensure this token is securely stored and managed.


### Docker Compose Setup


The provided docker-compose.yml sets up the following services:

Jenkins: Automation server for CI/CD pipelines.
SonarQube: Tool for continuous inspection of code quality.
SonarScanner: CLI tool to perform SonarQube analysis.
PostgreSQL: Database for SonarQube.

To start all services, run:

```bash
docker-compose up -d
```

This command will pull the necessary Docker images and start the containers in detached mode.



### Jenkins Pipeline
- The Jenkins pipeline is defined in the Jenkinsfile located at the root of the repository. This pipeline automates the following stages:

Checkout: Clones the PHP project from GitHub.
Install Dependencies: Uses Composer to install PHP dependencies.
Run Unit Tests: Executes PHPUnit tests.
SonarQube Analysis: Performs code quality analysis using SonarQube.



- Key Points:

- Agent: Runs the pipeline on any available Jenkins agent.
- Environment Variables:
- COMPOSER_HOME: Specifies the Composer home directory.
- SONAR_TOKEN: Retrieves the SonarQube token from Jenkins credentials.
- Stages:
Checkout: Clones the Symfony demo project from GitHub.
Install Dependencies: Uses Composer Docker image to install PHP dependencies.
Run Unit Tests: Executes PHPUnit tests within the Composer Docker container.
SonarQube Analysis: Runs SonarScanner to perform code quality checks.


### Usage
- Start Docker Services:

Ensure all Docker services are up and running.

```bash
docker-compose up -d
```

- Access Jenkins:

Open your browser and navigate to http://localhost:8080. Follow the initial Jenkins setup instructions.

- Configure Jenkins Credentials:

Navigate to Manage Jenkins > Manage Credentials.
Add a new credential with the ID jenkins-token containing your SonarQube token.
Create a New Pipeline Job:

In Jenkins, create a new pipeline job.
Point the pipeline to the Jenkinsfile in this repository.
- Run the Pipeline:

Trigger the pipeline manually or set up webhooks for automated triggers.

- Access SonarQube:

After the pipeline runs, access SonarQube at http://localhost:9000 to view the code quality reports.