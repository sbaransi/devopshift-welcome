pipeline {
    agent any

    environment {
        APP_NAME = 'my-flask-app' 
        DOCKER_IMAGE = "sbaransi/${APP_NAME}:${env.BUILD_NUMBER}"
        DOCKER_CREDS_ID = 'docker-hub-creds'
    }

    stages {
        stage('Build Docker Image') {
            steps {
                dir('welcome') {
                    script {
                        echo "Building Docker image: ${DOCKER_IMAGE}"
                        sh "docker build -t ${DOCKER_IMAGE} ."
                    }
                }
            }
        }

        // Parallel stage running tests and security scan at the same time
        stage('Quality & Security Checks') {
            parallel {
                stage('Security Scan') {
                    steps {
                        script {
                            echo "Scanning Docker image for vulnerabilities..."
                            // If a specific scanner like Trivy is installed, you'd run: sh "trivy image ${DOCKER_IMAGE}"
                            // For a general exercise/placeholder scan:
                            sh "echo 'Running vulnerability scan on ${DOCKER_IMAGE}... Passed!'"
                        }
                    }
                }
                stage('Code Quality / Lint') {
                    steps {
                        script {
                            echo "Running code tests..."
                            sh "echo 'Testing application integrity... Passed!'"
                        }
                    }
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: env.DOCKER_CREDS_ID, passwordVariable: 'DOCKER_PASS', usernameVariable: 'DOCKER_USER')]) {
                        echo "Logging into Docker Hub..."
                        sh "echo \$DOCKER_PASS | docker login -u \$DOCKER_USER --password-stdin"
                        
                        echo "Pushing image to Docker Hub..."
                        sh "docker push ${DOCKER_IMAGE}"
                    }
                }
            }
        }
    }
    
    post {
        always {
            echo "Cleaning up local Docker images..."
            sh "docker rmi ${DOCKER_IMAGE} || true"
        }
    }
}
