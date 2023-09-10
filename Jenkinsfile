pipeline {
    agent any
    
    stages {
        stage("Code") {
            steps {
                echo 'passed'
                // git url: "https://github.com/LondheShubham153/node-todo-cicd.git", branch: "master"
            }
        }
        
        stage("Build & Test") {
            steps {
                environment {
                    DOCKER_IMAGE = "thakur156/node-app-test:${BUILD_NUMBER}"
                }
                sh "docker build -t ${DOCKER_IMAGE} ."
            }
        }
        
        stage("Push to DockerHub") {
            environment {
                REGISTRY_CREDENTIALS = credentials('docker-cred')
            }
            steps {
                script {
                    def dockerImage = docker.image("${DOCKER_IMAGE}")
                    docker.withRegistry('https://index.docker.io/v1/', "docker-cred") {
                        dockerImage.push()
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

