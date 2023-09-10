pipeline {
    agent any
    
    stages{
        stage("Code"){
            steps{
                echo 'passed'
                //git url: "https://github.com/LondheShubham153/node-todo-cicd.git", branch: "master"
            }
        }
        stage("Build & Test"){
            steps{
                sh "docker build . -t node-app-test-new"
            }
        }
        stage("Push to DockerHub"){
            environment {
        REGISTRY_CREDENTIALS = credentials('docker-cred')
      }
            steps{
                docker.withRegistry('https://index.docker.io/v1/', "docker-cred") {
                sh "docker tag node-app-test-new thakur156/node-app-test-new:latest"
                    sh "docker push thakur156/node-app-test-new:latest" 
            }
                }
            }
        stage("Deploy"){
            steps{
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
