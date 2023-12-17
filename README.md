
#  Web application deploy into Kubernetes cluster

This mini project takes you on a journey through deploying a Node.js application on Amazon Web Services (AWS) using the Elastic Container Service (ECS) Fargate and Elastic Container Registry (ECR). We’ll explore the underlying technologies, tools, and steps required to achieve this feat.

AWS ECS Fargate: A serverless compute engine for containers that allows you to run containers without the need to manage the underlying infrastructure.

AWS ECR: A fully managed Docker container registry that makes it easy to store, manage, and deploy Docker container images.

## Steps
#### Step 1. AWS setup & cloning the source code from GitHub

Login to AWS Console and create an EC2 instance and allow port 8000 in inbound rules.

Name: ECR_ECS, AMI: Ubuntu 22.04, type:t2.micro > Launch Instance

Use name as you preferd i used nodecicd-server

```
git clone https://github.com/NirmalNaveen20/node-cicd.git
```

#### Step 2. Create IAM user and Installation of AWS CLI and Docker.

Write the policies required for accessing the ECR.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ecr:GetDownloadUrlForLayer",
                "ecr:BatchGetImage",
                "ecr-public:BatchCheckLayerAvailability",
                "ecr-public:PutImage",
                "ecr-public:DescribeRepositories",
                "ecr-public:GetRepositoryPolicy",
                "ecr:ListImages",
                "ecr-public:DeleteRepository",
                "ecr-public:CreateRepository",
                "ecr-public:GetAuthorizationToken",
                "sts:GetServiceBearerToken",
                "ecr-public:InitiateLayerUpload",
                "ecr-public:UploadLayerPart",
                "ecr-public:CompleteLayerUpload"
            ],
            "Resource": "*"
        }
    ]
}
```

User is created successfully but there is not any access key that key we have to Create.

    
Click on create access key.

Access key is created successfully done.

Install AWS CLI

`sudo apt-get install awscli`

Install Docker. [Docker installation doumentaion](https://docs.docker.com/engine/install/ubuntu/)

Add ubuntu user to docker group `sudo usermod -aG docker ubuntu`

Connect the EC2 instance with AWS management console through awscli.

`aws configure`

#### Step 3. Create container registry on ECR and Push docker image to ECR

* Go to AWS ECR in the AWS management console.

* Create a repository. I have selected the public repository and the repo name.

* Select all the operating systems and versions according to the OS of Fargate you would select. Finally, create the repository.

* Now go to instance and write a Dockerfile to build Docker image.

* Now, we will run the given commands that will be given in the “View Push Command” on the EC2 instance.

* Now Run first command to authenticate your docker client to your container registry.

* Push docker image to ECR repository

* After the image push, you can check the ECR for the image

* Build docker image with tag `docker build -t <tag-name> .`

#### Step 4. Create a Cluster in ECS.

* Navigate to the ECS service in the AWS console.

* Create a cluster in ECS.

* Provide the Cluster name, VPC and subnet you want your application to be available on.

We used to run docker run to create a container in the EC2 instance out of the docker image. That was nothing but a task. Therefore let’s create a task definition for our cluster.

Now, we will configure a task definition here, by providing the Task Name, Container Name, and URL of the image that is in the Repository and clicking on “Create”.

Now, click on “Create service”.

Choose the cluster. Select the launch type to be Fargate.

The task is now deployed to the cluster.

Navigate to the ENI ID in the task.

Go to the security group URL.

Navigate to the Inbound rule in the security group and open the Port 8000 which is HTTP and select my IP to have access for myself.

Copy the Public Ip of the Container with port 8000, and the site will be live.

⚠Note: 
Don’t forget to Stop or Delete all clusters and services of ECS otherwise this will cost you without knowing you.⚠

## Author

- [@Nirmal Naveen](https://www.nirmalnaveen.com/)

![Logo](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTlPjhPV6D68kBoBq82reUr6ndqcI_n9YPSQ9WA3sqT_RAXpDVcujzTO1MmWrcmcGYeyA&usqp=CAU)


