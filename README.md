# CloudBees-Demo

### 1. Launch and Configure CloudBees CI

Launch CloudBees CI (Enterprise version of Jenkins) using AWS QuickStart - https://aws.amazon.com/quickstart/architecture/cloudbees-ci/

Install recommended plugins 

Create a team master 
 > If not in CloudBees Team UI, click on the Teams link in the left menu.
 > Click on the Create team button in the center of your screen.
 > Name this team - enter a name for your team 
 > Choose an icon for this team to help uniquely identify your team - select an icon and color for your team and then click Next
 > Add people to this team - your user will show up as a Team Admin and we won’t be adding any additional users right now, but feel free to look around and then click Next.
 > Select the cluster endpoint to create the team in - just stick with the default value kubernetes and click Next.
 > Select team master creation recipe - click on the drop-down to see the options, but just stick with the Basic recipe.
 > Finally, click the Create team button

Install the below plug-ins by clicking on 'Manage Jenkins' option available in the left menu **when you click on the new team created in the above step**
 >  AWS CodeBuild 
 >  Pipeline: AWS Steps 
 >  AWS Global Configuration Plugin
 >  CloudBees AWS Credentials Plugin
 >  Pipeline

### 2. Create the CodeCommit Repository
 >  Create a CodeCommit repository in your AWS account 
 >  Check-in the files under sample-app folder in the repo
 >  Create Git credentials for HTTPS connections to CodeCommit - https://docs.aws.amazon.com/codecommit/latest/userguide/setting-up-gc.html#setting-up-gc-iam

### 3. Create the ECR Repo 
Create the ECR repository in your AWS account that will be used to store the Docker images 

### 4. Create the CodeBuild Project
 > Create a CodeBuild project in your AWS account using the latest AL2 Standard image. 
 > The source for this project will be the CodeCommit repo create in step 2. 
 > For the buildspec section, use the buildspec.yaml file from this repository. 
 > Also, create and initialize the below environment variables 
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
 > Create a parametrized Cloud CI pipeline which requests the following parameters
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
  > For the pipeline script, use the groovy script specified in the file Jenkinsfile

