#!/usr/bin/env groovy

pipeline{
    agent any
    tools{
        nodejs "node"
    }
     environment {
         IMAGE_NAME = 'santana20095/java-maven:1.0'
     }
    stages{
        
        stage('test'){
            steps{
                script{
                    echo "Testing pipeline..."
                }
            }
        }

        stage("build docker image"){
            steps{
                script{
                    echo "building the docker image..."
                    ssh "docker build  --platform linux/amd64 -t $IMAGE_NAME ."
                }
            }
        }

        stage("connect to EC2"){
            steps{
                script{
                    def runImage = "docker run -p 3080:3080 -d $IMAGE_NAME:1.0"
                    sshagent(["nodejs-app-ec2"]){
                        sh "ssh -o StrictHostKeyChecking=no ec2-user@13.39.146.56 "
                    }
                }
            }
        }
    }
}