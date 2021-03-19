# CloudBees-Demo

### 1. Launch and Configure CloudBees CI

Launch CloudBees CI (Enterprise version of Jenkins) using AWS QuickStart - https://aws.amazon.com/quickstart/architecture/cloudbees-ci/

Install recommended plugins 

Create a team master 
 1. If not in CloudBees Team UI, click on the Teams link in the left menu.
 2. Click on the Create team button in the center of your screen.
 3. Name this team - enter a name for your team. 
 4. Choose an icon for this team to help uniquely identify your team - select an icon and color for your team and then click Next.
 5. Add people to this team - your user will show up as a Team Admin and we won’t be adding any additional users right now, but feel free to look around and then click Next.
 6. Select the cluster endpoint to create the team in - just stick with the default value kubernetes and click Next.
 7. Select team master creation recipe - click on the drop-down to see the options, but just stick with the Basic recipe.
 8. Finally, click the Create team button

Install the below plugins by clicking on 'Manage Jenkins' option available in the left menu **when you click on the new team created in the above step**
 1. AWS CodeBuild 
 2. Pipeline: AWS Steps 
 3. AWS Global Configuration Plugin
 4. CloudBees AWS Credentials Plugin
 5. Pipeline

Some of the plugins may not show up in the 'Available Plugins' section and you may need to install the plugins using the hpi files for the plugins under 'Advanced'

### 2. Create the CodeCommit Repository
  1. Create a CodeCommit repository in your AWS account 
  2. Check-in the files under sample-app folder in the repo
  3. Create Git credentials for HTTPS connections to CodeCommit - https://docs.aws.amazon.com/codecommit/latest/userguide/setting-up-gc.html#setting-up-gc-iam

### 3. Create the ECR Repo 
Create the ECR repository in your AWS account that will be used to store the Docker images 

### 4. Create the CodeBuild Project
  1. Create a CodeBuild project in your AWS account using the latest AL2 Standard image. 
  2. The source for this project will be the CodeCommit repo create in step 2. 
  3. For the buildspec section, use the buildspec.yaml file from this repository. 
  4. Also, create and initialize the below environment variables 

    - AWS_DEFAULT_REGION 

    - AWS_ACCOUNT_ID

    - IMAGE_REPO_NAME

    - IMAGE-TAG 

### 4. Configure the CodeCommit, CodeBuild and AWS credentials in your Jenkins config 

### 5. Launch ECS Cluster via CloudFormation
Launch an ECS cluster in your AWS Account by using the file - ecs-cluster.yaml. This template uses an existing VPC (2 public and 2 private subnets) for the cluster. 

### 6. Create Load Balancer for ECS Service 
Launch an Application Load Balancer by using the file - ecs-cluster.yaml.

### 7. Create the CloudBees CI pipeline 
 1. Create a parametrized Cloud CI pipeline which requests the following parameters

    - CODEBUILD_PROJECT_NAME

    - REGION

    - CFN_STACK

    - TEMPLATE_FILE

    - ECS_CLUSTER

    - ECR_REPO 

    - DESIRED_TASK_COUNT 

    - VPC_ID 

    - ALB_LISTENER_ARN 

    - PRIORITY 

    - CONTAINER_PORT 

    - LB_NAME 

  2. For the pipeline script, use the groovy script specified in the file Jenkinsfile

