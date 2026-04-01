pipeline {
    agent any

    environment {
        IMAGE_NAME = "your-dockerhub-username/app"
        IMAGE_TAG  = "build-${BUILD_NUMBER}"
    }

    stages {
        stage('Clone') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
            }
        }

        stage('Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                    sh "docker push ${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }

        stage('Deploy') {
            steps {
                // Replace with your EC2 host and key credential
                sh "ssh -o StrictHostKeyChecking=no ec2-user@your-ec2-ip 'docker pull ${IMAGE_NAME}:${IMAGE_TAG} && docker stop app || true && docker rm app || true && docker run -d --name app -p 80:80 ${IMAGE_NAME}:${IMAGE_TAG}'"
            }
        }
    }
}
