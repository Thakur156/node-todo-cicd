pipeline {
    agent {
        docker {
            // Use an official Node.js image as the build environment
            image 'node:14'
            args '-u root' // You may need root permissions for certain operations
        }
    }

    environment {
        // Define environment variables here, e.g., for secrets and configurations
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout your code from your version control system (e.g., Git)
                // Example for Git:
                // checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                // Install Node.js dependencies using npm or yarn
                sh 'npm install'  // Replace with your package manager if needed
            }
        }

        stage('Code Quality') {
            steps {
                // Run code quality checks (e.g., ESLint for linting, and Mocha for unit tests)
                sh 'npm run lint'    // Replace with your linting command
                sh 'npm test'         // Replace with your test command
            }
        }

        stage('Security Scanning') {
            steps {
                // Run security scanning tools (e.g., OWASP Dependency-Check)
                sh 'npm audit'        // Replace with your security scanning command
            }
        }

        stage('Docker Build and Push') {
            steps {
                // Build and push the Docker image
                script {
                    def dockerImage = 'thakur156/nodejs:latest'

                    // Build the Docker image
                    sh "docker build -t ${dockerImage} ."

                    // Authenticate with your container registry (e.g., Docker Hub)
                    withCredentials([usernamePassword(
                        credentialsId: 'docker-cred',
                        usernameVariable: 'DOCKER_USERNAME',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )]) {
                        sh "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"
                    }

                    // Push the Docker image to the registry
                    sh "docker push ${dockerImage}"
                }
            }
        }

        stage('Build and Deploy') {
            steps {
                // Build and deploy your Node.js application (e.g., to a server or container)
                // Replace with your deployment commands
            }
        }

        stage('Notify') {
            steps {
                // Notify the team or perform any post-build actions here
            }
        }
    }

    post {
        success {
            // Add actions to perform on a successful build
        }
        failure {
            // Add actions to perform on a failed build
        }
    }
}


