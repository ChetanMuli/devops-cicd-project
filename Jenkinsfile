pipeline {

    agent any

    environment {
        IMAGE_NAME = "chetanmuli/devops-cicd"
    }

    stages {

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

        stage('Manual Approval') {
            steps {
                input 'Deploy to EC2?'
            }
        }

        stage('Deploy to EC2') {
    steps {
        sh """
        ssh -o StrictHostKeyChecking=no ubuntu@16.16.200.91 '
        sudo docker pull chetanmuli/devops-cicd
        sudo docker stop devops-container || true
        sudo docker rm devops-container || true
        sudo docker run -d -p 80:3000 --name devops-container chetanmuli/devops-cicd
        '
        """
    }
}
    }

}
