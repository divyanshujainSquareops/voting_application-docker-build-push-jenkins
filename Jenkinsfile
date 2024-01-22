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
        IMAGE_TAG = "${DOCKER_HUB_REPO}/votingapp-worker:${BUILD_NUMBER}"
    }

    stages {
        stage('Clone in Kaniko Container') {
            steps {
                script {
                    container('kaniko') {
                        git branch: 'main', credentialsId: 'github', url: 'https://github.com/divyanshujainSquareops/voting_application-helm-argoCd-jenkins.git'
                        echo "Repository cloned inside Kaniko container"
                    }
                }
            }
        }

        stage('Build and Push Docker Images') {
            steps {
                script {
                    container('kaniko') {
                        // Build and push Docker image using Kaniko
                        sh "/kaniko/executor --dockerfile ./result/Dockerfile --context \$(pwd) --destination ${IMAGE_TAG}"
                        
                        // Login to Docker Hub
                        sh "docker login -u divyanshujain11 -p Deepu@123#"
                        
                        // Push the image
                        sh "docker push ${IMAGE_TAG}"
                        
                        // Remove local images
                        sh "docker rmi -f \$(docker images -a -q)"
                    }
                }
            }
        }
    }
}
