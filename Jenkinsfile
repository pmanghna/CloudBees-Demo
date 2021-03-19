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
                cfnUpdate(stack:"$CFN-STACK", file:"$TEMPLATE-FILE", params: ['ECRTag':"${env.VERSION}",'ECSCluster':"$ECS-CLUSTER",'ECRAppRepoName':"$ECR-REPO",'DesiredTaskCount':"$DESIRED-TASK_COUNT",'VpcId':"$VPC-ID",'ALBListener':"$ALB-LISTENER-ARN",'Priority':"$PRIORITY",'ContainerPort':"$CONTAINER-PORT",'AppLoadBalancerName':"prod-ALB"],timeoutInMinutes:10)
        }
        
          }
         }
        }
      }
