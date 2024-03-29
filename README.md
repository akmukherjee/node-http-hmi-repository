# Udacity Cloud DevOps Capstone Project 

This is the final Capstone Cloud DevOps Project

## Project Overview

In this project, a simple NodeJS application is deployed using JenkinsX and Kubernetes. A full CI/CD Pipeline has be created which first lints and then builds the NodeJS code base and then if the build is successful, the code first gets deployed to Staging and then if approved it gets promoted to the Production environment. The ecosystem and its components are as shown below.

<p align="center"> 
<img src="images/UdacityCloudDevOps1.jpg">
</p>

## Project Steps

###  Cluster Creation

The project was created on the Google Cloud Platform due to known issues with running JenkinsX. JenkinsX was first installed on a GCP Virtual Machine and subsequently a GCP Cluster was created as shown below. The Cluster contains a 3 Worker Nodes. The cluster was created using the command 
**jx create cluster gke --skip-login**

<p align="center"> 
<img src="images/K8sClusterNodes.jpg">
</p>

###  Codebase Creation

A Simple NodeJS application was created using JenkinsX which displays a page as shown below and runs on the K8s Cluster created in the previous step and displays a simple Hello Node message. 


<p align="center"> 
<img src="images/NodeJSApp.jpg">
</p>

### Pipeline Creation

The above steps result in creation of a simple Pipeline which creates a Docker Image and upon successfult build uploads the image to DockerHub and deploys it to the Staging Environment first. 


<p align="center"> 
<img src="images/PipelinePreLint.jpg">
</p>

### Deployment Strategy

A Canary strategy is used here where if the deployment works on the Staging Environment it gets deployed to the Production Environment. A rolling deployment strategy is used and the pods. When a build works successfully on the staging environment it can be promoted to the production environment using the command **jx promote --version 0.0.7 --env production** and the pods get a rolling update and the environment gets updated as can be seen from the screenshot of production cluster after deployment. 


<p align="center"> 
<img src="images/PipelineProductionPostDeploy.jpg">
</p>




### Addition of a Lint Step

A Linting Step has been added to the pipeline to lint the NodeJS application. The lint uses a .esrclint.json file which provides basic linting rules and updates to the package.json file to add the npm command and to the Jenkins file to add a linting stage. After the linting stage the updated pipeline looks as below.

<p align="center"> 
<img src="images/PipelinePostLint.jpg">
</p>

### Lint Error Demonstration

A Linting Error is deliberately introduced by tweaking the server.js file and as shown below the pipeline fails due to the error generated as a result.

<p align="center"> 
<img src="images/PipelineErrorLint.jpg">
</p>

### Pull Request Flow

When a branch is created and a Pull Request generated, the PR kicks off the build and pushes the docker image to Docker Hub as shown below. It does not deploy it to the staging environment. That happens ONLY when the PR is merged into the Master.

<p align="center"> 
<img src="images/PRFlow.jpg">
</p>