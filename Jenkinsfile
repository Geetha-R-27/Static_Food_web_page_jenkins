pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = 'dockerhub-credential'  // Jenkins stored credentials ID
        DOCKERHUB_USERNAME = 'geethar27'
        IMAGE_NAME = 'static-webpage'
        IMAGE_TAG = 'latest'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Cloning repository...'
                git branch: 'main', url: 'https://github.com/Geetha-R-27/Static_Food_web_page_jenkins.git'
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: "${DOCKERHUB_CREDENTIALS}", usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh """
                    # Pull base nginx image
                    docker pull nginx:alpine

                    # Create temporary container
                    docker create --name temp-nginx nginx:alpine

                    # Copy all files from repo into nginx html folder
                    docker cp . temp-nginx:/usr/share/nginx/html/

                    # Commit the container as new image
                    docker commit temp-nginx ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:${IMAGE_TAG}

                    # Login and push to Docker Hub
                    echo \$PASSWORD | docker login -u \$USERNAME --password-stdin
                    docker push ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:${IMAGE_TAG}

                    # Cleanup temporary container
                   
                    """
                }
            }
        }
    }

    post {
        success {
            echo "Docker image ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:${IMAGE_TAG} pushed successfully!"
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
