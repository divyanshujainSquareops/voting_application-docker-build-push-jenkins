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
                        sh "cat /kaniko/.docker/config.json
                    }
                }
            }
        }

        // stage('Add Docker Hub Credentials') {
        //     steps {
        //         script {
        //             container('kaniko') {
        //                 // Use Jenkins credentials to set up Docker Hub credentials
        //                 withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_LOGIN', passwordVariable: 'DOCKER_PASSWORD')]) {
        //                     sh  '''
        //                         echo "{\"auths\":{\"https://index.docker.io/v1/\":{\"auth\":\"$(echo -n "$DOCKER_LOGIN:$DOCKER_PASSWORD" | base64)\"}}}" > /kaniko/.docker/config.json
        //                     '''
        //                     echo "Docker Hub credentials added"
        //                 }
        //             }
        //         }
        //     }
        // }

        // stage('Build and Push Docker Images') {
        //     steps {
        //         script {
        //             container('kaniko') {
        //                 // Build and push Docker image using Kaniko
        //                 sh '''
        //                     /kaniko/executor --dockerfile result/Dockerfile \
        //                     --context=`pwd` \
        //                     --destination=${DOCKER_HUB_REPO}/votingapp-resul:${BUILD_NUMBER} \
        //                     --skip-tls-verify \
        //                     --docker-config=/kaniko/.docker/config.json
        //                 '''
        //                 echo "Image build completed"
        //             }
        //         }
        //     }
        // }
    }
}
