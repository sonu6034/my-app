pipeline {
    agent any

    environment {
        IMAGE_NAME = "sonu6034/my-app"
        DOCKER_CREDENTIALS_ID = "dockerhub"
    }

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/sonu6034/my-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t $IMAGE_NAME:$BUILD_NUMBER .'
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: "${DOCKER_CREDENTIALS_ID}", usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    script {
                        sh """
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push $IMAGE_NAME:$BUILD_NUMBER
                        docker tag $IMAGE_NAME:$BUILD_NUMBER $IMAGE_NAME:latest
                        docker push $IMAGE_NAME:latest
                        """
                    }
                }
            }
        }
    }
}

