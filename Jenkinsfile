pipeline {
    agent {
        kubernetes {
            label 'kaniko'
            yaml """
apiVersion: v1
kind: Pod
metadata:
    name: kaniko
spec:
    restartPolicy: Never
    containers:
    - name: kaniko
      image: gcr.io/kaniko-project/executor:debug
      command:
      - /busybox/cat
      tty: true
"""
        }
    }

    environment {
        registryCredential = 'dockerhub' 
        DOCKER_HUB_REPO = 'divyanshujain11'
    }

    stages {
        stage('Clone in Kaniko Container') {
            steps {
                container('kaniko') {
                   script {
                        // Use the checkout step to clone the repository
                        checkout([$class: 'GitSCM', branches: [[name: 'main']], userRemoteConfigs: [[url: 'https://github.com/divyanshujainSquareops/voting_application-helm-argoCd-jenkins.git', credentialsId: 'github']]])
                        echo "Repository cloned inside Kaniko container"
                    }
                }
            }
        }

        stage('Build and Push Docker Images') {
            steps {
                container('kaniko')  {
                    script {
                        sh "ls"
                    }
                }
            }
        }
    }
}
