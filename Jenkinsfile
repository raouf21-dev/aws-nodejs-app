#!/usr/bin/env groovy

pipeline{
    agent any
    tools{
        nodejs "node"
    }
     environment {
         IMAGE_NAME = 'santana20095/aws-nodejs-app:1.0'
     }
    stages{
        
        stage('test'){
            when {
            expression {
              return env.BRANCH_NAME == "main"
            }
            } 
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
                    sh "docker build -t $IMAGE_NAME ."
                }
            }
        }

        stage("push docker image"){
            steps{
                script{
                    echo "Pushing Image..."
                    withCredentials([usernamePassword(credentialsId: "docker-hub-creds", passwordVariable: "PASS", usernameVariable: "USER")]){
                        sh "echo $PASS | docker login -u $USER --password-stdin"
                        sh "docker push $IMAGE_NAME" 
                    }
                }
            }
        }

        stage("connect to EC2"){
            steps{
                script{
                    def runImage = "docker run -p 3000:3000 -d $IMAGE_NAME"
                    sshagent(["nodejs-app-ec2"]){
                        sh "scp -o StrictHostKeyChecking=no server-cmds.sh ec2-user@13.39.146.56:/home/ec2-user/"
                        sh "scp -o StrictHostKeyChecking=no docker-compose.yaml ec2-user@13.39.146.56:/home/ec2-user/"

                        sh """
                            ssh -o StrictHostKeyChecking=no ec2-user@13.39.146.56 '
                                docker pull $IMAGE_NAME && 
                                sh /home/ec2-user/server-cmds.sh
                            '
                         """
                    }
                }
            }
        }
    }
}