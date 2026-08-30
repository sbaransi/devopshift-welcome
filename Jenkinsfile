pipeline {
    agent any

    environment {
        APP_NAME = 'my-flask-app' 
        
        // IMPORTANT: Change 'yourdockerhubuser' to your actual Docker Hub username!
        DOCKER_IMAGE = "sbaransi/${APP_NAME}:${env.BUILD_NUMBER}"
        
        DOCKER_CREDS_ID = 'docker-hub-creds'
    }

    stages {
        stage('Build Docker Image') {
            steps {
                // This tells Jenkins to enter the 'welcome' folder before building
                dir('welcome') {
                    script {
                        echo "Building Docker image: ${DOCKER_IMAGE}"
                        sh "docker build -t ${DOCKER_IMAGE} ."
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
