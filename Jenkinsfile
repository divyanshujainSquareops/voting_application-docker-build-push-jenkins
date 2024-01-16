pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                dir('~/') {
                    git branch: 'main', credentialsId: 'git_hub', url: 'https://github.com/divyanshujainSquareops/voting_application-helm-argoCd-jenkins.git'
                }
            }
        }
        stage('Build and Push Docker Images') {
            steps {
                script {
                    sh "git clone https://github.com/divyanshujainSquareops/voting_application-helm-argoCd-jenkins.git"
                }
            }

        
    }
}
