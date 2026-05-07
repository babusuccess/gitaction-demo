pipeline {
    agent any

    stages {

        stage('Docker Build') {
            steps {
                sh 'docker build -t YOUR_DOCKERHUB_USERNAME/myapp:latest .'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh '''
                        echo "$PASS" | docker login -u "$USER" --password-stdin
                    '''
                }
            }
        }

        stage('Docker Push') {
            steps {
                sh 'docker push YOUR_DOCKERHUB_USERNAME/myapp:latest'
            }
        }
    }
}
