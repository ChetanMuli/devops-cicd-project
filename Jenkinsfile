pipeline {

    agent any

    environment {
        IMAGE_NAME = "yourdockerhubusername/devops-cicd"
    }

    stages {

        stage('Clone Code') {
            steps {
                git 'https://github.com/yourgithubusername/devops-cicd-project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                credentialsId: 'dockerhub',
                usernameVariable: 'USER',
                passwordVariable: 'PASS'
                )]) {

                sh 'docker login -u $USER -p $PASS'

                }
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push $IMAGE_NAME'
            }
        }

        stage('Deploy to EC2') {
            steps {
                sh '''
                ssh ubuntu@EC2_PUBLIC_IP << EOF
                docker pull yourdockerhubusername/devops-cicd
                docker stop devops-container || true
                docker rm devops-container || true
                docker run -d -p 80:3000 --name devops-container yourdockerhubusername/devops-cicd
                EOF
                '''
            }
        }

    }
}