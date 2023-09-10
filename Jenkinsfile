pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = "" // Define DOCKER_IMAGE at the top level
    }
    
    stages {
        stage("Code") {
            steps {
                echo 'passed'
                // git url: "https://github.com/Thakur156/node-todo-cicd.git", branch: "master"
            }
        }
        
        stage("Build & Test") {
            steps {
                script {
                    DOCKER_IMAGE = "thakur156/node-app-test:${BUILD_NUMBER}"
                    sh "docker build -t ${DOCKER_IMAGE} ."
                }
            }
        }
        
        stage("Push to DockerHub") {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-cred', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    script {
                        def dockerImage = docker.image("${DOCKER_IMAGE}")
                        docker.withRegistry('https://index.docker.io/v1/', "docker-cred") {
                            dockerImage.push()
                        }
                    }
                }
            }
        }
        
        stage("Deploy") {
            steps {
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
