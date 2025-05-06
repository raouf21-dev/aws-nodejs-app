#!/usr/bin/env groovy

pipeline{
    agent any
    tools{
        nodejs "node"
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
                    ssh "docker build  --platform linux/amd64 -t $imageName ."
                }
            }
        }
        stage("connect to EC2"){
            steps{
                script{
                    def runImage = "docker run -p 3080:3080 -d santana20095/react-nodejs:1.1"
                    sshagent(["nodejs-app-ec2"]){
                        sh "ssh -o StrictHostKeyChecking=no ec2-user@13.39.146.56 ${runImage}"
                    }
                }
            }
        }
    }
}