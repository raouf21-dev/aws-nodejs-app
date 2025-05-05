#!/usr/bin/env groovy

pipeline{
    agent any
    stages{
        
        stage('test'){
            steps{
                script{
                    echo "Testing pipeline..."
                }
            }
        }
        stage("connect to EC2"){
            steps{
                script{
                    sshagent("nodejs-app-ec2")
                        sh "ssh -o StrictHostKeyChecking=no ec2-user@13.39.146.56 docker ps"
                }
            }
        }
    }
}