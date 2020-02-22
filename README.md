# Udacity Cloud Devops Final Project - Capstone
Final submission of my Cloud Devops Project - Capstone

## Project Scope

The scope of this project is to apply my collective skills and knowledge to create a CI/CD blue/green pipeline. 

I've decided to follow the project suggestion of deploying an 'index.html' file using Nginx.



![pipeline](images/pipeline.png)

### Step 1: Propose and Scope the Project
- Pipeline will be: GitHub -> Jenkins -> Docker -> Kubernetes
	- Push new code to repo
	- Jenkin lints all files in the repo
	- If successful, then jenkins will build a docker image
	- Deploy the docker image to dockerhub
	- Image updates on Kubernetes cluster?

- Decide which options you will include in your Continuous Integration phase.
- Use Jenkins - possible Jenkins or Jenkins X
- I will use the blue/green deployment found [HERE](https://medium.com/@andresaaap/simple-blue-green-deployment-in-kubernetes-using-minikube-b88907b2e267)
- Nginx “Hello World, my name is (student name)” application from chapter ?

### Step 2: Use Jenkins, and implement blue/green or rolling deployment.
- Create your Jenkins master box with either Jenkins and install the plugins you will need.
- Set up your environment to which you will deploy code.

### Step 3: Pick AWS Kubernetes as a Service, or build your own Kubernetes cluster.
- Use Ansible or CloudFormation to build your “infrastructure”; i.e., the Kubernetes Cluster.
- It should create the EC2 instances (if you are building your own), set the correct networking settings, and deploy software to these instances.
- As a final step, the Kubernetes cluster will need to be initialized. The Kubernetes cluster initialization can either be done by hand, or with Ansible/Cloudformation at the student’s discretion.

### Step 4: Build your pipeline
- Construct your pipeline in your GitHub repository.
- Set up all the steps that your pipeline will include.
- Configure a deployment pipeline.
- Include your Dockerfile/source code in the Git repository.
- Include with your Linting step both a failed Linting screenshot and a successful Linting screenshot to show the Linter working properly.

### Step 5: Test your pipeline
- Perform builds on your pipeline.
- Verify that your pipeline works as you designed it.
- Take a screenshot of the Jenkins pipeline showing deployment and a screenshot of your AWS EC2 page showing the newly created (for blue/green) or modified (for rolling) instances. Make sure you name your instances differently between blue and green deployments.


## Project Submission Checklist:
### Zip with the following files:

#### Screenshots
- Failed Linting of HTML
- Successful Linting of HTML
- Jenkins pipeline showing deployment 
- Screenshot of my AWS EC2 page showing the newly created (for blue/green)

#### Text Files
- Text file with link to Github 


Push new code to the front-end folder
Jenkins lints all html files in the folder
Successful linting causes jenkins to build a docker image
Deploy docker image to dockerhub
Trigger a rolling update on kubernetes cluste

## notes to delete
username for jenkins: frankbaileyjr


## Jenkins Plugins Installed
- BlueOcean
- Config API for Blue Ocean
- Events API for Blue Ocean
- Git Pipeline for Blue Ocean
- GitHub Pipeline for Blue Ocean
- Blue Ocean Pipeline Editor
- Display URL for Blue Oean
- Blue Ocean Executor Info

# Other software
- pip install awscli --upgrade
- docker --version = 18.09.7 
- sudo snap install kubectl --classic

# errors ran into 
- sudo chmod 666 /var/run/docker.sock 
- Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Permission Denied error



# after creating cluster, updated to 1.14 in order manage the node































After thorough search, I decided to use Jenkins-X due to its native support for Kubernetes and I also wanted to experiment. Due to the simplicity of the application, I found it easier to use rolling deployment; any change is directly applied to the application. 

I followed the setup instructions for AWS (described in <https://aws.amazon.com/blogs/opensource/continuous-delivery-eks-jenkins-x/>), I was able to install the cluster by executing the command

```javascript
jx create cluster eks --cluster-name=dimicloud --skip-installation=true
```

but the next step

```
jx install --provider=eks --default-environment-prefix=dimicloud
```

was never completing. I googled the issue and other people had the same issue without any solution existing. After spending a significant amount of time, as a result, I decided to switch to Google Cloud Platform (GCP), despite the fact that I didn't have any experience with this platform. The issue, is that some features that I used in AWS in the previous project such as linting, I was not able to implement in GCP. 

Starting over with creating a new cluster executing the command:

```
jx create cluster gke --min-num-nodes=2 --max-num-nodes=2 --skip-login
```

and it was competed successfully deploying two nodes and returning the following log file:

```
our Kubernetes context is now set to the namespace: jx
To switch back to your original namespace use: jx namespace default
Or to use this context/namespace in just one terminal use: jx shell
For help on switching contexts see: https://jenkins-x.io/developing/kube-context/
To import existing projects into Jenkins:       jx import
To create a new Spring Boot microservice:       jx create spring -d web -d actuator
To create a new microservice from a quickstart: jx create quickstart
Fetching cluster endpoint and auth data.
kubeconfig entry generated for shiftlake.
Context "gke_stellar-orb-126418_europe-west1-b_shiftlake" modified.
NAME              HOSTS                                      ADDRESS          PORTS   AGE
chartmuseum       chartmuseum.jx.35.187.176.187.nip.io       35.187.176.187   80      5m35s
docker-registry   docker-registry.jx.35.187.176.187.nip.io   35.187.176.187   80      5m36s
jenkins           jenkins.jx.35.187.176.187.nip.io           35.187.176.187   80      5m36s
nexus             nexus.jx.35.187.176.187.nip.io             35.187.176.187   80      5m36s
```

![nodes](images/nodes.png)



## Jenkins-X installation

Jenkins-X comes with some project templates. Since the application I wanted to use is similar to the one of the templates that creates a docker image running php, I created that template and modified the docker image accordingly. Doing this process the code scafolding is implemented, as shown in the github <https://github.com/dmavridis/Udacity-DevOps>

The initial webpage looks like the screenshot. 

![version1](images/version1.png)

Doing a small change in the html file, after a few minutes a new version is up as shown in the following screenshots:







