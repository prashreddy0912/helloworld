pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "falskapp"
        EC2_USER = "ec2-user"
        // EC2_HOST = "your.ec2.public.ip"
        EC2_HOST = "13.200.195.68"
        // SSH_KEY = "/path/to/private-key.pem"
        SSH_KEY = "C:\Users\15769\Downloads\rudra-keypair.pem"
    }
  stages {
        stage('Clone Repository') {
            steps {
                checkout scm
            }
        }
        stage('Build & Push Docker Image') {
            steps {
                sh "docker build -t $DOCKER_IMAGE ."
                sh "docker login -u prashreddy0912 -p qwertyuiop"
                sh "docker push $DOCKER_IMAGE"
            }
        }

        stage('Deploy to EC2') {
            steps {
                sh """
                ssh -i $SSH_KEY $EC2_USER@$EC2_HOST '
                docker stop flask_app || true
                docker rm flask_app || true
                docker pull $DOCKER_IMAGE
                docker run -d -p 5020:5020 --name flask_app $DOCKER_IMAGE
                '
                """
            }
        }
    }
}
