pipeline {
    agent any

    environment {
        IMAGE_NAME = "sonu6034/my-app"
        IMAGE_TAG = "latest"
    }

    stages {
        stage('Clone Code') {
            steps {
                git 'https://github.com/sonu6034/my-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t $IMAGE_NAME:$IMAGE_TAG ."
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh "echo $PASSWORD | docker login -u $USERNAME --password-stdin"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh "docker push $IMAGE_NAME:$IMAGE_TAG"
            }
        }

        stage('Run Container') {
            steps {
                script {
                    sh "docker stop my-app || true"
                    sh "docker rm my-app || true"
                    sh "docker run -d -p 5000:5000 --name my-app $IMAGE_NAME:$IMAGE_TAG"
                }
            }
        }
    }
}
