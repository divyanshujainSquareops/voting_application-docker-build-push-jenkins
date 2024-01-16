pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Clone the GitHub repository
                    git branch: 'main', credentialsId: 'git_hub', url: 'https://github.com/divyanshujainSquareops/voting_application-helm-argoCd-jenkins.git'
                }
            }
        }

        // Add more stages as needed

    }
}
