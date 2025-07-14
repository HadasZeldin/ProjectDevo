pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'hadashub/flask-app'
        DOCKER_TAG = 'latest'
    }

    stages {
        stage('checkout') {
            steps {
               sh 'git clone https://github.com/HadasZeldin/ProjectDevo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
               sh 'docker build -t hadashub/flask-app:latest ./ProjectDevo/flask-app .'

            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push $DOCKER_IMAGE:latest
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f deployment/deployment.yaml'
            }
        }
    }
}
