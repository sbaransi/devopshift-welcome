pipeline {
    agent any

    environment {
        // Using ENV for application name (Instructor Requirement)
        APP_NAME = 'my-flask-app' 
        
        // Using application name for docker image name & BUILD_NUMBER for tag (Instructor Requirement)
        // IMPORTANT: Change 'yourdockerhubuser' to your actual Docker Hub username!
        DOCKER_IMAGE = "sbaransi/${APP_NAME}:${env.BUILD_NUMBER}"
        
        // The ID of the credential we stored in Jenkins in Step 2
        DOCKER_CREDS_ID = 'docker-hub-creds'
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building Docker image: ${DOCKER_IMAGE}"
                    // Jenkins automatically downloads the code from Git before this step begins
                    sh "docker build -t ${DOCKER_IMAGE} ."
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    // Using Jenkins credentials to upload image securely (Instructor Requirement)
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
