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
        COMPOSE_FILE_NAME = 'docker-compose.yml'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/divyanshujainSquareops/voting_application-helm-argoCd-jenkins.git'
                echo "clone complete"
            }
        }

        stage('Build_and_Push_Docker_Images') {
            steps {
                script {
                    // // Assuming docker-compose file is in the root directory
                    // sh "docker-compose -f ${COMPOSE_FILE_NAME} build"
                    
                    // // Push each image with the new tag
                    // sh "docker tag workspace_worker ${DOCKER_HUB_REPO}/votingapp-worker:${BUILD_NUMBER}"
                    // sh "docker tag workspace_result ${DOCKER_HUB_REPO}/votingapp-result:${BUILD_NUMBER}"
                    // sh "docker tag workspace_vote ${DOCKER_HUB_REPO}/votingapp-vote:${BUILD_NUMBER}"
                    
                    // // Push only the new images
                    // sh "docker login -u divyanshujain11 -p Deepu@123#"
                    // sh "docker push ${DOCKER_HUB_REPO}/votingapp-worker:${BUILD_NUMBER}"
                    // sh "docker push ${DOCKER_HUB_REPO}/votingapp-result:${BUILD_NUMBER}"
                    // sh "docker push ${DOCKER_HUB_REPO}/votingapp-vote:${BUILD_NUMBER}"
                    // sh "docker rmi -f \$(docker images -a -q)"
                }
            }
        }
    }
}
