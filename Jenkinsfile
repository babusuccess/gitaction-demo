pipeline {
    agent any

    environment {
        IMAGE_NAME = "babupalavesam/myapp:latest"
    }

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/babusuccess/gitaction-demo.git'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-cred', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                }
            }
        }

        stage('Docker Push') {
            steps {
                sh 'docker push $IMAGE_NAME'
            }
        }
    }
}
