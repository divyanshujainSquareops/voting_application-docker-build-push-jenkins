pipeline {
    agent any

    environment {
        DOCKER_HUB_REPO = 'divyanshujain11'
        COMPOSE_FILE_NAME = 'docker-compose.yml'
    }

    stages {
        stage('Checkout') {
            steps {
                dir('~/') {
                    git "https://github.com/divyanshujainSquareops/voting_application-helm-argoCd-jenkins.git"
                }
            }
        }

        stage('Build and Push Docker Images') {
            steps {
                script {
                    // Assuming docker-compose file is in the root directory or specified directory
                    sh "docker-compose -f ./${COMPOSE_FILE_NAME} build"

                    // Log in to Docker Hub
                    withDockerRegistry([credentialsId: 'dockerhub', url: 'https://index.docker.io/v1/']) {
                        // Push all images with the new tag
                        sh "docker-compose -f ./${COMPOSE_FILE_NAME} config --services | xargs -I {} docker tag {} ${DOCKER_HUB_REPO}/votingapp-{}:${BUILD_NUMBER}"
                        sh "docker-compose -f ./${COMPOSE_FILE_NAME} config --services | xargs -I {} docker push ${DOCKER_HUB_REPO}/votingapp-{}:${BUILD_NUMBER}"
                    }
                }
            }
        }
    }
}
