# Project Name: SWE645 Instructions file for extended installation steps and supported screenshots

## Description

This README provides step-by-step instructions for setting up a development and deployment environment for your project. The environment involves technologies like Docker, Kubernetes, Jenkins, and GitHub. 

# Table of Contents

- [Installation and Setup](#installation-and-setup)
- [Ubuntu Server and Docker](#ubuntu-server-and-docker)
- [Kubernetes Cluster Setup](#kubernetes-cluster-setup)
- [Automated Build and Release with Jenkins](#automated-build-and-release-with-jenkins)

### Demo Videos
#### Installation video
https://drive.google.com/file/d/1HSFAQOaUX285cRCGmbiPx5dzLrO-d7JC/view?usp=sharing

#### Demo Video
https://drive.google.com/file/d/12jGbqji73piYlNwunUmf0P5Qeth064ZC/view?usp=sharing


## Installation and Setup

- **GitHub Repository :** Commit code for HW1 on a github repository. [GitHub repository](https://github.com/adi-limbekar/SWE-645-Assignment-2).

- **Docker Hub Repository:** A Docker Hub repository for the .war file is available at [Docker Hub Repository](https://hub.docker.com/repository/docker/adi0222/surveyformimage/general).

## Ubuntu Server and Docker

1. Start an EC2 instance using an Ubuntu AMI. You can access the Rancher Dashboard [here](https://ec2-18-208-33-134.compute-1.amazonaws.com/dashboard/home).

2. Install Docker on the Ubuntu instance.

3. Run Rancher on the EC2 instance. You can verify that Rancher is running by executing `sudo docker ps`.

4. Access the Rancher dashboard using the public DNS address of the EC2 instance.

5. Create a custom cluster within Rancher for your application.

## Kubernetes Cluster Setup

1. Create another EC2 instance, ensuring it has at least 25GB of storage, and use an Ubuntu AMI. This instance will serve as your Kubernetes server. You can access it at [K8s server](https://ec2-35-170-98-81.compute-1.amazonaws.com/).

2. After installing Docker on the new EC2 instance, register the nodes for your Kubernetes cluster using the provided command. This instance will host the etcd, control panel, and worker nodes.

3. Once the Kubernetes cluster is active, create two deployments (one with NodePort and one with LoadBalancer).

4. Download the KubeConfig file for your cluster and save it.

5. Provide the Docker repository address for the deployment to pull the image and deploy the application.

6. Verify that the pods are created and have a status of "running."

7. Check the logs for the pods using the command `kubectl logs <pod_name>` to ensure the .war file was successfully loaded.

8. You should be able to access the "Student Survey" form from the deployment services address.

## Automated Build and Release with Jenkins

1. Create another EC2 instance to host Jenkins. You can access Jenkins at [Jenkins](http://3.85.248.232:8080/).

2. Install Java on this instance and then install Jenkins from [https://pkg.jenkins.io/debian/](https://pkg.jenkins.io/debian/).

3. Login to Jenkins UI then: Manage Jenkins -> Credentials(Under Security) -> Click on (global) under Domains -> Add Credentials

4. To add kubeconfig.yaml, Kind = Secret file, File = Attach your .yaml file downloaded from Rancher, ID = You can give any name or it will be auto generated.
   
5. We also added credentials for DockerHub follow same steps as point 3, which we used in our Jenkins file.
   
6. Create a Jenkins pipeline and install the necessary plugins, including [CloudBees Docker](https://docs.cloudbees.com/docs/admin-resources/latest/plugins/docker-workflow) and [Rancher](https://plugins.jenkins.io/rancher).
   
7. Configure the pipeline with the source as your GitHub repository. Enable polling of the SCM every minute and provide access to the JenkinsFile stored in your GitHub repository.
    
8. Create and push a 'JenkinsFile' that consists of stages, including:
   a. Build.
   b. Push to Docker Hub.
   c. Deploy on Rancher single node.
   d. Deploy on Rancher.

9. Once the pipeline is successfully set up, any changes to the code pushed to GitHub will trigger a new build. After completing all the pipeline stages, the new build will be pushed to Docker Hub, and the image will be updated in Rancher deployments, now hosting the newly created build.


