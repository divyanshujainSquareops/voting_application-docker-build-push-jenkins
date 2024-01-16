pipeline {
    agent any

    environment {
        DOCKER_HUB_REPO = 'divyanshujain11'
        COMPOSE_FILE_NAME = 'docker-compose.yml'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', credentialsId: 'git_hub', url: 'https://github.com/divyanshujainSquareops/voting_application-helm-argoCd-jenkins.git'
            }
        }

        stage('Build_and_Push_Docker_Images') {
            steps {
                script {
                    // Assuming docker-compose file is in the root directory
                    sh "docker-compose -f ./${COMPOSE_FILE_NAME} build"

                    // Log in to Docker Hub
                    withDockerRegistry([credentialsId: 'dockerhub', url: 'https://index.docker.io/v1/']) {
                        // Push each image with the new tag
                        sh "docker tag workspace_worker ${DOCKER_HUB_REPO}/votingapp-worker:${BUILD_NUMBER}"
                        sh "docker tag workspace_result ${DOCKER_HUB_REPO}/votingapp-result:${BUILD_NUMBER}"
                        sh "docker tag workspace_vote ${DOCKER_HUB_REPO}/votingapp-vote:${BUILD_NUMBER}"
                        
                        sh "docker push ${DOCKER_HUB_REPO}/votingapp-worker:${BUILD_NUMBER}"
                        sh "docker push ${DOCKER_HUB_REPO}/votingapp-result:${BUILD_NUMBER}"
                        sh "docker push ${DOCKER_HUB_REPO}/votingapp-vote:${BUILD_NUMBER}"
                    }
                }
            }
        }
    }
}
