pipeline {
    agent any

    parameters {
        string(name: 'VERSION', defaultValue: 'v1', description: 'Docker Image Version Tag')
    }

    environment {
        IMAGE_NAME = "babugyadav/nginx"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/babusuccess/gitaction-demo.git'
            }
        }

        stage('Docker Build') {
            steps {
                sh "docker build -t $IMAGE_NAME:${params.VERSION} ."
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-cred',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                }
            }
        }

        stage('Docker Push') {
            steps {
                sh "docker push $IMAGE_NAME:${params.VERSION}"
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh """
                sed -i 's|image: .*|image: $IMAGE_NAME:${params.VERSION}|g' deployment.yaml
                kubectl apply -f deployment.yaml
                """
            }
        }

        stage('Restart Deployment') {
            steps {
                sh "kubectl rollout restart deployment babu-app"
            }
        }
    }
}
