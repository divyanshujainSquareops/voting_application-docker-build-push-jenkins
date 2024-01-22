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
        DOCKER_LOGIN='divyanshujain11'
        DOCKER_PASSWORD='Deepu@123#'
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
              stage('Add Docker Hub Credentials') {
            steps {
                container('kaniko') {
                    script {
                        // Run the commands to add Docker Hub credentials
                        sh '''
                            mkdir -p /kaniko/.docker
                            ./config.sh
                            mv config.json /kaniko/.docker
                        '''
                        echo "Docker Hub credentials added"
                    }
                }
            }
        }
        stage('Build and Push Docker Images') {
            steps {
                container('kaniko')  {
                    script {
                            sh '''
                                /kaniko/executor --dockerfile `pwd`/result/Dockerfile \
                                --context=`pwd`/result \
                                --destination=${DOCKER_HUB_REPO}/votingapp-result:${BUILD_NUMBER} 
                               '''
                echo "image build"
                    }
                }
            }
        }
    }
}
