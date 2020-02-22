# Udacity Cloud Devops Final Project - Capstone
This is my final submission of the Cloud Devops Engineer Project - Capstone

## Project Scope

The scope of this project is to apply my collective skills and knowledge to create a CI/CD blue/green pipeline. 

I've decided to follow the project suggestion of deploying an 'index.html' file using Nginx.

-------

## Step 1: Propose and Scope the Project
- Pipeline will be: GitHub -> Jenkins -> Docker -> Kubernetes
	- Push new code to repo
	- Jenkin lints all files in the repo
	- If successful, then jenkins will build a docker image
	- Deploy the docker image to dockerhub
	- Image updates on Kubernetes cluster

- I will use the blue/green deployment found [HERE](https://medium.com/@andresaaap/simple-blue-green-deployment-in-kubernetes-using-minikube-b88907b2e267)
- Pulling a Nginx image and displaying the index.html file

## Step 2: I'm using the standard Jenkins in a blue/green deployment
- I created an EC2 instance in us-east-2 that will be my Jenkins server
- In order for the Jenkins environment work correctly, I had to install the following packages/modules:
	
###	***Jenkins Plugins Installed***
	- BlueOcean
	- Config API for Blue Ocean
	- Events API for Blue Ocean
	- Git Pipeline for Blue Ocean
	- GitHub Pipeline for Blue Ocean
	- Blue Ocean Pipeline Editor
	- Display URL for Blue Oean
	- Blue Ocean Executor Info

### Ubuntu packages
	- tidy
	- awscli 1.18.5
	- docker --version = 18.09.7 
	- kubectl

-------


### Step 3: Create the "Infrastruture"
- Used CloudFormation to build my Kubernetes Cluster
- I had to initialize my the Kubernetes cluster

### Step 4: Build your pipeline
- Constructed my pipeline to use my [github repo](https://github.com/fbaileyjr/udacity_cloud_devops_capstone) to pull in the files
- Configureed the deployment pipeline.
- My Dockerfile is included in the Git repo
- Before creating and pushing the Docker images, linting is performed on the index.html to verify the syntax is correct.
	- If successful, goes to the next step
	- If not, then the build fails

### Step 5: Test your pipeline
- Tested the pipelines
	- Test 1: Edited the index.html with bad code, then pushed it up to my repo. The build failed due to the linting failure.
	- Test 2: Edited the index.html with proper code, then pushed it up to my repo. The build completed successfully.

------

## Problems Encountered 
- I had to run the following command to resolve the docker error listed below: "sudo chmod 666 /var/run/docker.sock"
```
Permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Permission Denied
```
- After creating cluster in EKS, updated to 1.14 in order manage the nodes (original was 1.13)
- I found the installed awscli on EC2 was version 1.14, which did NOT have the EKS command. 
My pipeline would fail on the "upload latest green deployment to AWS Loadbalancer" step due the missing EKS command. 
The EKS command was added in version 1.15+. This meant I had to update the AWSCLI

------

## Project Submission Checklist:
### A Zip file with the following files:

#### Screenshots
- [X] Failed Linting of HTML 
- [X] Successful Linting of HTML
- [X] Jenkins pipeline showing deployment process 
- [X] Screenshot of my AWS EC2 page showing the newly created (for blue/green)

#### Text Files
- Text file with link to Github 

The ZIP file was submitted on FEB-22-2020

