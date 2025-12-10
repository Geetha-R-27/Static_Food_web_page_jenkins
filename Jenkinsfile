pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = 'dockerhub-credential'  // Jenkins stored credentials ID
        DOCKERHUB_USERNAME = 'geethar27'
        IMAGE_NAME = 'static-webpage'
        IMAGE_TAG = 'latest'
    }

    tools{
        maven 'maven''
    }
    stages {

        stage('Checkout') {
            steps {
                echo 'Cloning repository...'
                git 'https://github.com/yourusername/your-repo.git'  // Replace with your repo
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                script {
                    docker.build("${DOCKERHUB_USERNAME}/${IMAGE_NAME}:${IMAGE_TAG}")
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: "${DOCKERHUB_CREDENTIALS}", usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh "echo $PASSWORD | docker login -u $USERNAME --password-stdin"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                echo 'Pushing image to Docker Hub...'
                sh "docker push ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:${IMAGE_TAG}"
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            sh 'docker logout'
        }
        success {
            echo "Docker image ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:${IMAGE_TAG} pushed successfully!"
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
