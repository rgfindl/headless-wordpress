# Headless Wordpress Deployment using AWS CloudFormation
Quickly spin up a headless Wordpress deployment using AWS CloudFormation.

[![](diagram1.png)](diagram1.png)


[![](diagram2.png)](diagram2.png)

Infrastructure as Code (IaC) is the recommended way to manage the cloud infrastructure that your
application runs on.  IaC allows you to incrementally add/remove infrastructure as your application changes.

IaC really shines when you need to spin up a new environment.  Lets say you get a huge customer who wants
to be on their own instance.  You can be up in running within the hour.

This project contains 5 CloudFormation scripts.  They must be created in order because they depend on each other:
1. VPC
2. ECS
3. RDS
4. ECR
5. Wordpress

# Prerequisites
- [AWS Account](https://aws.amazon.com/)
- [EC2 Key Pair](https://console.aws.amazon.com/ec2/v2/home)
- cim - (`npm install -g cim`)
- [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/installing.html) 


# Stacks

## VPC
This creates the [Amazon Virtual Private Cloud](https://aws.amazon.com/vpc/) that our ECS cluster and RDS database will run in.  
> Amazon Virtual Private Cloud (Amazon VPC) lets you provision a logically isolated section of the AWS Cloud where you can launch AWS resources in a virtual network that you define. 
```
cd vpc
cim stack-up
```

## ECS
This creates an [Elastic Container Service](https://aws.amazon.com/ecs/) that our EC2's will run in.
> Amazon Elastic Container Service (Amazon ECS) is a highly scalable, high-performance container orchestration service that supports Docker containers and allows you to easily run and scale containerized applications on AWS.
```
cd vpc
cim stack-up
```

## RDS
This creates a [Relational Database Service](https://aws.amazon.com/rds/) database cluster that our Wordpress application will use.
> Amazon Relational Database Service (Amazon RDS) makes it easy to set up, operate, and scale a relational database in the cloud.
```
cd rds
export DatabaseUsername="???"; export DatabasePassword="???"; cim stack-up
```

## ECR
This creates an [Elastic Container Registry](https://aws.amazon.com/ecr/) that will hold the docker images of our Wordpress service.
> Amazon Elastic Container Registry (ECR) is a fully-managed Docker container registry that makes it easy for developers to store, manage, and deploy Docker container images.
```
cd ecr
cim stack-up
```

### Wordpress
Before we can launch this cloudformation stack.  We need to push our service image to ECR.
#### Push Image
```
cd wordpress/src
```
- [Registry Authentication](http://docs.aws.amazon.com/AmazonECR/latest/userguide/Registries.html#registry_auth)
  - `aws ecr get-login --registry-ids <account-id>`
  - copy/past output to perform docker login,  also append `/headless-wp` to the repository url.
- Build Image
  - `docker build -t headless-wp:<version> .`
- [Push Image](http://docs.aws.amazon.com/AmazonECR/latest/userguide/docker-push-ecr-image.html)
  - `docker tag headless-wp:<version> <account-id>.dkr.ecr.<region>.amazonaws.com/headless-wp:latest`
  - `docker tag headless-wp:<version> <account-id>.dkr.ecr.<region>.amazonaws.com/headless-wp:<version>`
  - `docker push <account-id>.dkr.ecr.<region>.amazonaws.com/headless-wp`

#### Update Version
Make sure the `Version` parameter, in _cim.yml, matches the `version` tag from above.  The ECS Task Definition will pull the image from ECR.

#### Stack up
Once the `Version` is set you can use `cim stack-up` to update the stack with the new version.

```
cd wordpress
cim stack-up
```

# Wordpress
Congratulations, your new Wordpress site is now available.  

First run through the Wordpress setup wizard.

Next enable some of the plugins we added.

Add a few blog posts and pages.

Then check out the API. Ex: `https://<cdn-url>/wp-json/wp/v2/posts`

# Tear down
```
cd wordpress
cim stack-delete

cd ecr
cim stack-delete

cd rds
cim stack-delete

cd ecs
cim stack-delete

cd vpc
cim stack-delete
```
