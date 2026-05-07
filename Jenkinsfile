pipeline {

    agent any

    environment {

        IMAGE_NAME = "babugyadav/nginx:mytestimage"
        TAG = "mytestimage"
    }

    stages {

        stage('Clone Code') {

            steps {

                git 'https://github.com/babusuccess/gitaction-demo.git'
            }
        }

        stage('Docker Build') {

            steps {

                sh 'docker build -t $IMAGE_NAME:$TAG .'
            }
        }

        stage('Docker Login') {

            steps {

                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {

                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                }
            }
        }

        stage('Docker Push') {

            steps {

                sh 'docker push $IMAGE_NAME:$TAG'
            }
        }

        stage('Kubernetes Deploy') {

            steps {

                sh 'kubectl apply -f deployment.yaml'
            }
        }

        stage('Check Pods') {

            steps {

                sh 'kubectl get pods'
            }
        }
    }
}
