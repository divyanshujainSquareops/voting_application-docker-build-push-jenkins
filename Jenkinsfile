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

        
    }
}
