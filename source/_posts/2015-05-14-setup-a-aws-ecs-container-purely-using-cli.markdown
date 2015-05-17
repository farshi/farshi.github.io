---
layout: post
title: "Setup an AWS ECS container purely using CLI"
date: 2015-05-14 23:03:10 +1000
comments: true
categories: [AWS ECS, DOCKER Container, AMI , IAM , Task Definition, ECS optimized IAM ]
---

In this post I’m trying to setup an ECS container in a custom cluster with one docker instance inside by using AWS CLI. The  [AWS user guides](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/ECS_AWSCLI.html) partially describes how to setup an EC2 Container service using the AWS CLI but in some steps It leaves some tricks to the reader, for example user have to login to AWS console to setup EC2 machine needed to act as an ECS machine or in one of the steps you have to login and create needed IAM role and required policies to assign to the EC2 machine. 
<!--more-->
In this post I walk through you to setup everything purely by AWS CLI commands from start to finish. It includes:

* Configuring AWS CLI environment on the user machine.
* Creating key pair which is need to connect to the EC2 machine.
* Creating the instance of EC2 machine.
* Creating the required IAM role and in-line attached policies to assign to the EC2.
* Configuring EC2 machine to be assigned to custom cluster instead of default one.
* Defining and running a Task.

 
Let’s get started with configuring the AWS CLI environment on your machine.

## Step 1 - configure keys

``` objc
 ~/ aws configure
AWS Access Key ID []:****************6HEQ
AWS Secret Access Key []:****************mFbV
Default region name []: ap-southeast-2
Default output format []:Json
```
I choose ap-southeast-2 , coz I am using Sydney region.

## Step 2: Create a Cluster

```
 ~/ aws ecs create-cluster --cluster-name MyCluster
{
    "cluster": {
        "status": "ACTIVE",
        "clusterName": "MyCluster",
        "registeredContainerInstancesCount": 0,
        "pendingTasksCount": 0,
        "runningTasksCount": 0,
        "clusterArn": "arn:aws:ecs:ap-southeast-2:338764315603:cluster/MyCluster"
    }
}
```
 
## Step 3: Launch an Instance with the Amazon ECS AMI

This step includes some sub steps.

### Step 3-1 : Creating a Key Pair

To create a key pair named MyKeyPair, use the create-key-pair command, and use the `--query` option and the `--output` text option to pipe your private key directly into a file.

```
 ~/ aws ec2 create-key-pair --key-name MyKeyPair --query 'KeyMaterial' --output text > MyKeyPair.pem

```

If you're using an SSH client on a Linux computer to connect to your instance, use the following command to set the permissions of your private key file so that only you can read it.

` ~/ chmod 400 MyKeyPair.pem`

To displaying Your Key Pair to verify you have done the key pair creation.

```
 ~/ aws ec2 describe-key-pairs --key-name MyKeyPair
{
    "KeyPairs": [
        {
            "KeyName": "MyKeyPair",
            "KeyFingerprint": "df:6a:55:cc:66:4f:2e:a3:8b:ca:67:16:be:b9:f5:4a:db:5a:26:1a"
        }
    ]
}
```
### Step 3-2 : Creating a Security Group

Containers associated with your tasks can map their network ports to ports on the host Amazon ECS container instance so they are reachable from the Internet. If your container has external connectivity, then your container instance security group must allow inbound access to the ports you want to expose.

Here we don’t want to have a docker instance to communicate with outside but I open the port 22 coz later we might want to login into the machine and inspect AWS ecs-agent and our created docker instance. AWS ecs-agent is a docket instance by itself. In the real production environment it’s not a good practice to open ssh port on the ECS machine but if you wan to do it you should choose restricted CIDR instead of 0.0.0.0/0.

```
 ~/ aws ec2 create-security-group --group-name MySecurityGroup --description "My security group”
 ~/ aws ec2 authorize-security-group-ingress --group-name MySecurityGroup --protocol tcp --port 22 --cidr 0.0.0.0/0
 ~/  aws ec2 describe-security-groups --group-names MySecurityGroup
```

### Step 3-3: Creating userdata.txt to specify ClusterName

By default, your container instance launches into your default cluster. If you want to launch into your own cluster instead of the default, replacing 'your_cluster_name' with the name of your cluster.

`#!/bin/bash 
echo ECS_CLUSTER=your_cluster_name >> /etc/ecs/ecs.config`

Create a file `userdata.txt` and paste these lines into it:

```
#!/bin/bash 
echo ECS_CLUSTER=MyCluster >> /etc/ecs/ecs.config
```

### Step 3-4 : Creating required instance profile, role and policy

In this step we want to create the required role with inline policy and assign the role to the instance profile. Because the Amazon ECS container agent makes calls to Amazon ECS on your behalf, you need to launch container instances with an IAM role that authenticates to your account and provides the required resource permissions.  


Create a new file `ecsPolicy.json` using nano or vim and put following lines into the file:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "",
      "Effect": "Allow",
      "Principal": {
        "Service": "ec2.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

Create a new file `rolePolicy.json` and put the following lines into the file:

```
 {
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ecs:CreateCluster",
        "ecs:DeregisterContainerInstance",
        "ecs:DiscoverPollEndpoint",
        "ecs:Poll",
        "ecs:RegisterContainerInstance",
        "ecs:Submit*"
      ],
      "Resource": [
        "*"
      ]
    }
  ]
}
```

And then issue these set of commands:

```
 ~/.aws/ aws iam create-role --role-name ecsRole --assume-role-policy-document file://ecsPolicy.json

 ~/.aws/ aws iam put-role-policy --role-name ecsRole --policy-name ecsRolePolicy  --policy-document file://rolePolicy.json

 ~/.aws/ aws iam add-role-to-instance-profile --instance-profile-name ecsRole  --role-name ecsRole
```

### Step 3-5 : Running EC2 instance on a default VPC

The Amazon ECS container agent allows container instances to connect to your cluster. The Amazon ECS container agent is included in the Amazon ECS-optimized AMI, but you can also install it on any EC2 instance that supports the Amazon ECS specification. The Amazon ECS container agent is only supported on EC2 instances.

For finding an AMI which is ECS optimized login into the AWS console then from the console dashboard, choose Launch Instance. On the Choose an Amazon Machine Image (AMI) page, choose Community AMIs. Choose an AMI for your container instance.  you can replace image-id options by  the image-id you find.

I choose this AMI :
` amzn-ami-2015.03.b-amazon-ecs-optimized - ami-39017e03 `

So now we can pass security group and IAM instance profile that we created in previous steps.

```
 ~/ aws ec2 run-instances   --image-id ami-39017e03 --count 1 --instance-type t2.micro --key-name MyKeyPair --iam-instance-profile "Name= ecsRole" --security-groups MySecurityGroup --user-data file://userdata.txt
```

### Step 3-6: Adding a Name Tag to Your Instance

```
 ~/ aws ec2 create-tags --resources i-fcb33a20 --tags Key=Name,Value=MyECSInstance
```

## Step 4:  List Container Instances

by issuing this command you can see the container instances list. Because we configure ace-agent to be deployed in MyCluster, for listing 
the container instance it needs cluster be specified.

```
 ~/ aws ecs list-container-instances --cluster MyCluster
{
    "containerInstanceArns": [
        "arn:aws:ecs:ap-southeast-2:338764315603:container-instance/3de53ed6-5017-46fa-9b9f-37ad8a2b9da6"
    ]
}
```


## Step 5: Describe your Container Instance

To list the details of containers in our cluster we can use following commands. The details of containers , amount of CPU , memory capacity and also network ports will be shown.  Container-instances which listed for you by the previous command should be passed to this command.

```
 ~/ aws ecs describe-container-instances --cluster MyCluster --container-instances 3de53ed6-5017-46fa-9b9f-37ad8a2b9da6
```

## Step 6: Register a Task Definition

For defining the task we need to create a JSON file and specify the amount of memory , number of CPU units, the name of the task , the container image we want to use , and the type of the container.

`~/ nano sleep360.json`
``` 
{
  "containerDefinitions": [
    {
      "name": "sleep",
      "image": "busybox",
      "cpu": 10,
      "command": [
        "sleep",
        "360"
      ],
      "memory": 10,
      "essential": true
    }
  ],
  "family": "sleep360"
}
```

Here our defined task is essential and it means the cluster will be died if this container dies. our task is a simple task which wants to run a bash command  'sleep 360'  and the container will be terminated after running the command. exactly after 6 minutes. :D

by issuing the following command and passing the created file to the command we be able to register our task definition. 

``` 
 ~/ aws ecs register-task-definition --cli-input-json file://sleep360.json
{
    "taskDefinition": {
        "volumes": [],
        "taskDefinitionArn": "arn:aws:ecs:ap-southeast-2:338764315603:task-definition/sleep360:1",
        "containerDefinitions": [
            {
                "environment": [],
                "name": "sleep",
                "mountPoints": [],
                "image": "busybox",
                "cpu": 10,
                "portMappings": [],
                "command": [
                    "sleep",
                    "360"
                ],
                "memory": 10,
                "essential": true,
                "volumesFrom": []
            }
        ],
        "family": "sleep360",
        "revision": 1
    }
}
```

To be sure that our task is registered, issuing this command will list the defined tasks.

``` 
 ~/  aws ecs list-task-definitions
{
    "taskDefinitionArns": [
        "arn:aws:ecs:ap-southeast-2:338764315603:task-definition/sleep360:1"
    ]
}
```
## Step 7: Run a Task
 
To running a task simple use this command. By passing the count argument we can specify how many instance of container spin up on the cluster.

```
 ~/ aws ecs run-task --cluster MyCluster --task-definition sleep360:1 --count 1
```
 
To listing the task use the following command.

```
 ~/ aws ecs list-tasks --cluster MyCluster
{
    "taskArns": [
        "arn:aws:ecs:ap-southeast-2:338764315603:task/7cceb4d5-8950-42a3-923e-b1859d401399"
    ]
}
```
By passing the task ARN (listed above) to the following command we can see details of the running task.

```
 ~/ aws ecs describe-tasks --cluster MyCluster --task 7cceb4d5-8950-42a3-923e-b1859d401399
{
    "failures": [],
    "tasks": [
        {
            "taskArn": "arn:aws:ecs:ap-southeast-2:338764315603:task/7cceb4d5-8950-42a3-923e-b1859d401399",
            "overrides": {
                "containerOverrides": [
                    {
                        "name": "sleep"
                    }
                ]
            },
            "lastStatus": "RUNNING",
            "containerInstanceArn": "arn:aws:ecs:ap-southeast-2:338764315603:container-instance/3de53ed6-5017-46fa-9b9f-37ad8a2b9da6",
            "clusterArn": "arn:aws:ecs:ap-southeast-2:338764315603:cluster/MyCluster",
            "desiredStatus": "RUNNING",
            "taskDefinitionArn": "arn:aws:ecs:ap-southeast-2:338764315603:task-definition/sleep360:1",
            "containers": [
                {
                    "containerArn": "arn:aws:ecs:ap-southeast-2:338764315603:container/d440587d-b23b-45f3-a2a8-ff8b195d0240",
                    "taskArn": "arn:aws:ecs:ap-southeast-2:338764315603:task/7cceb4d5-8950-42a3-923e-b1859d401399",
                    "name": "sleep",
                    "networkBindings": [],
                    "lastStatus": "RUNNING",
                    "exitCode": 0
                }
            ]
        }
    ]
}
```

## Step 8 (Final): SSH into the EC2 machine and list running container

In the final step  we want to ssh into the machine and list the containers inside the machine.

ssh -i MyKeyPair.pem ec2-user@EC2-Public-IP-ADDRESS

after login into the machine, you can issue  `packer -ps`  command to list the containers. you should see the  `ecs-agent` and  `sleep 360`

```
[ec2-user@ip-xxx-xxx-xxx-xx ~]$ docker ps
CONTAINER ID        IMAGE                            COMMAND             CREATED             STATUS                  PORTS                        NAMES
95d32fb0a1a2        busybox:latest                   "sleep 360"         1 seconds ago       Up Less than a second                                ecs-sleep360-1-sleep-fec1a8f1fbfd9d82d601
0ad94851d185        amazon/amazon-ecs-agent:latest   "/agent"            11 minutes ago      Up 11 minutes           127.0.0.1:51678->51678/tcp   ecs-agent

```

Hope you enjoy this post and hope it helps you to understand how to use the ECS service using AWS CLI.


