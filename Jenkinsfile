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
                    sh "docker login -u ${env.DOCKER_USERNAME} -p ${env.DOCKER_PASSWORD}"
                    sh "docker push ${DOCKER_IMAGE}"
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

