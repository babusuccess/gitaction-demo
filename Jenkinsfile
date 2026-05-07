pipeline {

    agent any

    environment {
        IMAGE_NAME = "babugyadav/nginx"
        TAG = "v1"
    }

    stages {

        stage('Clone Code') {
            steps {
                git url: 'https://github.com/babusuccess/gitaction-demo.git', branch: 'main'
            }
        }

        stage('Docker Build') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:${TAG} ."
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
                sh "docker push ${IMAGE_NAME}:${TAG}"
            }
        }

        stage('Kubernetes Deploy') {
            steps {
                sh "kubectl apply -f deployment.yaml"
            }
        }

        stage('Check Pods') {
            steps {
                sh "kubectl get pods"
            }
        }
    }

    post {
        success {
            echo "Pipeline SUCCESS 🚀 Application deployed successfully"
        }

        failure {
            echo "Pipeline FAILED ❌ Check logs"
        }
    }
}
