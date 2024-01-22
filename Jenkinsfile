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
                   script  {
                        git branch: 'main', credentialsId: 'github', url: 'https://github.com/divyanshujainSquareops/voting_application-helm-argoCd-jenkins.git'
                        echo "Repository cloned inside Kaniko container"
                    }
                }
            }
        }

        stage('Build and Push Docker Images') {
            steps {
                container('kaniko')  {
                        script{
                                        sh '''
                    /kaniko/executor --dockerfile `pwd`/result/Dockerfile \
                    --context=`pwd` \
                    --destination=${DOCKER_HUB_REPO}/votingapp-result:${BUILD_NUMBER} 
                '''
                echo "image build"
                    }
                }
            }
        }
    }
}
