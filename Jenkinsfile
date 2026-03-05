pipeline {

    agent any

    environment {
        IMAGE_NAME = "chetanmuli/devops-cicd"
    }

    stages {

        stage('Clone Code') {
            steps {
                git branch: 'main', url: 'https://github.com/ChetanMuli/devops-cicd-project.git'
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
                usernameVariable: 'chetanmuli13@gmail.com',
                passwordVariable: 'Chetan@12'
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
                ssh ubuntu@16.170.108.127 << EOF
                docker pull chetanmuli/devops-cicd
                docker stop devops-container || true
                docker rm devops-container || true
                docker run -d -p 80:3000 --name devops-container chetanmuli/devops-cicd
                EOF
                '''
            }
        }

    }
}
