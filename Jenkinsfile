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
      volumeMounts:
      - name: kaniko-secret
        mountPath: /kaniko/.docker  
    volumes:
    - name: kaniko-secret
      secret:
        secretName: kaniko-secret
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
                script {
                    container('kaniko') {
                        git branch: 'main', credentialsId: 'github', url: 'https://github.com/divyanshujainSquareops/voting_application-helm-argoCd-jenkins.git'
                        echo "Repository cloned inside Kaniko container"
                    }
                }
            }
        }
        
        stage('list all files') {
            steps {
                script {
                    container('kaniko') {
                        sh "ls"
                        sh "ls /kaniko/.docker"
                        sh "cat /kaniko/.docker/config.json"
                    }
                }
            }
        }

        stage('Build and Push Docker Images') {
            steps {
                script {
                    container('kaniko') {
                        // Build and push Docker image using Kaniko
                        sh '''
                            /kaniko/executor --dockerfile `pwd`/result/Dockerfile \
                            --context=`pwd` \
                            --destination=${DOCKER_HUB_REPO}/votingapp-result:${BUILD_NUMBER} 
                        '''
                        echo "Image build completed"
                    }
                }
            }
        }
    }
}
