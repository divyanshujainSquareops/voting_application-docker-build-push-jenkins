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
             
        stage('Build and Push Docker Images') {
            steps {
                container('kaniko')  {
                    script {
                            sh """
                        docker run \
                            -v \${PWD}:/workspace \
                            -v \${JENKINS_HOME}/secrets/docker-config.json:/kaniko/.docker/config.json \
                            gcr.io/kaniko-project/executor:latest \
                            --dockerfile `pwd`/result/Dockerfile \
                            --context `pwd`/result \
                            --destination ${DOCKER_HUB_REPO}/votingapp-worker:${BUILD_NUMBER}
                    """
                echo "image build"
                    }
                }
            }
        }
    }
}
