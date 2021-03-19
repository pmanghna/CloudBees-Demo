#!/usr/bin/env groovy
import jenkins.model.*

pipeline {
    agent any

    stages {
      stage ('ECS-Deployment - Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '<codecommit-credentials-id>', url: "$GIT-REPO-HTTPS-URL"]]])
        }
       }
    
    
      stage('ECS-Deployment - Build') {
            steps {
                awsCodeBuild projectName: "$CODEBUILD_PROJECT_NAME", region: "$REGION", sourceControlType: "project", credentialsType: 'jenkins', credentialsId: '<codebuild-credentials-id>'
            }
        }
     
       stage ('ECS-Deployment - Deploy') {
      
            steps {
                withAWS( credentials: 'aws-credentials', region: "$REGION" ) {
                s3Download(file:'tagValue.txt', bucket:'buildartifacts-sample-app', path:'artifacts/tagValue.txt',force:true)
                script {
                env.VERSION = readFile 'tagValue.txt'
                }
                cfnUpdate(stack:"$CFN_STACK", file:"$TEMPLATE_FILE", params: ['ECRTag':"${env.VERSION}",'ECSCluster':"$ECS_CLUSTER",'ECRAppRepoName':"$ECR_REPO",'DesiredTaskCount':"$DESIRED_TASK_COUNT",'VpcId':"$VPC_ID",'ALBListener':"$ALB_LISTENER_ARN",'Priority':"$PRIORITY",'ContainerPort':"$CONTAINER_PORT",'AppLoadBalancerName':"$LB_NAME"],timeoutInMinutes:10)
        }
        
          }
         }
        }
      }
