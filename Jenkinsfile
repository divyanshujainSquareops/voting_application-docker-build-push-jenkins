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
    volumes:
    - name: kaniko-secret
      secret:
            secretName: kaniko-secret
    containers:
    - name: kaniko
      image: gcr.io/kaniko-project/executor:debug
      command:
        - /busybox/cat
      tty: true
      volumeMounts:
      - name: kaniko-secret
        mountPath: /kaniko/.docker
    - name: kaniko-2
      image: gcr.io/kaniko-project/executor:debug
      command:
        - /busybox/cat
      tty: true
      volumeMounts:
      - name: kaniko-secret
        mountPath: /kaniko/.docker
    - name: kaniko-3
      image: gcr.io/kaniko-project/executor:debug
      command:
        - /busybox/cat
      tty: true
      volumeMounts:
      - name: kaniko-secret
        mountPath: /kaniko/.docker
"""
        }
    }

    environment {
        DOCKER_HUB_REPO = 'divyanshujain11'
    }

    stages {
        stage('Clone in Kaniko Container 1') {
            steps {
                script {
                    container('kaniko') {
                        git branch: 'main', credentialsId: 'github', url: 'https://github.com/divyanshujainSquareops/voting_application-helm-argoCd-jenkins.git'
                        echo "Repository cloned inside Kaniko container"
                    }
                }
            }
        }

        stage('Clone in Kaniko Container 2') {
            steps {
                script {
                    container('kaniko-2') {
                        git branch: 'main', credentialsId: 'github', url: 'https://github.com/divyanshujainSquareops/voting_application-helm-argoCd-jenkins.git'
                        echo "Repository cloned inside Kaniko container"
                    }
                }
            }
        }

        stage('Clone in Kaniko Container 3') {
            steps {
                script {
                    container('kaniko-3') {
                        git branch: 'main', credentialsId: 'github', url: 'https://github.com/divyanshujainSquareops/voting_application-helm-argoCd-jenkins.git'
                        echo "Repository cloned inside Kaniko container"
                    }
                }
            }
        }

        stage('Build and Push result Images') {
            steps {
                script {
                    container('kaniko') {
                        // Build and push Docker result image using Kaniko
                        sh "/kaniko/executor --dockerfile `pwd`/result/Dockerfile --context=`pwd` --destination=${DOCKER_HUB_REPO}/votingapp-result:${BUILD_NUMBER}"
                        echo "result Image build completed"
                    }
                }
            }
        }

        stage('Build and Push vote Images') {
            steps {
                script {
                    container('kaniko') {
                        // Build and push Docker image using Kaniko
                        sh "/kaniko/executor --dockerfile `pwd`/vote/Dockerfile --context=`pwd` --destination=${DOCKER_HUB_REPO}/votingapp-vote:${BUILD_NUMBER}"
                        echo "Image vote build completed"
                    }
                }
            }
        }

        stage('Build and Push seed Images') {
            steps {
                script {
                    container('kaniko-2') {
                        // Build and push Docker image using Kaniko
                        sh "/kaniko/executor --dockerfile `pwd`/seed-data/Dockerfile --context=`pwd` --destination=${DOCKER_HUB_REPO}/votingapp-seed-data:${BUILD_NUMBER}"
                        echo "seed-data Image build completed"
                    }
                }
            }
        }

        stage('Build and Push worker Images') {
            steps {
                script {
                    container('kaniko-3') {
                        // Build and push Docker image using Kaniko
                        sh "/kaniko/executor --dockerfile `pwd`/worker/Dockerfile --context=`pwd` --destination=${DOCKER_HUB_REPO}/votingapp-worker:${BUILD_NUMBER}"
                        echo "worker Image build completed"
                    }
                }
            }
        }
    }
}
