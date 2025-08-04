pipeline {
    agent any
    environment {
        IMAGE_NAME = "sonu6034/my-app"
        DOCKER_CREDENTIALS_ID = "dockerhub"
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
                    sh "docker build -t $IMAGE_NAME:latest ."
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS_ID) {
                        sh "docker push $IMAGE_NAME:latest"
                    }
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    sh "docker rm -f my-app || true"
                    sh "docker run -d --name my-app -p 5555:5000 $IMAGE_NAME:latest"
                }
            }
