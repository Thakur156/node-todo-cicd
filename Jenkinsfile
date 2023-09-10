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
            steps{
                withCredentials([usernamePassword(credentialsId:"docker-cred",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                    sh "docker login -u ${env.dockerHubUser} -password-stdin ${env.dockerHubPass}"
                    sh "docker tag node-app-test-new ${env.dockerHubUser}/node-app-test-new:latest"
                    sh "docker push ${env.dockerHubUser}/node-app-test-new:latest" 
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
