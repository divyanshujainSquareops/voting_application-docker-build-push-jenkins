pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                dir("${HOME}") {
                    git branch: 'main', credentialsId: 'git_hub', url: 'https://github.com/divyanshujainSquareops/voting_application-helm-argoCd-jenkins.git'
                }
            }
        }
        
    }
}
