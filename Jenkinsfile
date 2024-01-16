pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/divyanshujainSquareops/voting_application-helm-argoCd-jenkins.git'
        CREDENTIALS_ID = 'git_hub'
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    dir("${HOME}") {
                        checkout([$class: 'GitSCM', branches: [[name: 'main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: "${CREDENTIALS_ID}", url: "${REPO_URL}"]]])
                    }
                }
            }
        }

        // Add more stages as needed

    }
}
