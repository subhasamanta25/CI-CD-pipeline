pipeline {
    agent any

    environment {
        IMAGE_NAME = "subha250504/flask-app"
        IMAGE_TAG = "latest"
    }

    stages {

        stage('Checkout') {
            steps {
                echo "Checking out source code..."
                checkout scm
            }
        }

        stage('Test') {
            steps {
                echo "Installing dependencies and running tests..."
                sh '''
                    python3 -m pip install --upgrade pip
                    pip3 install -r requirements.txt
                    pytest
                '''
            }
        }

        stage('Build Docker') {
            steps {
                echo "Building Docker image..."
                sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
            }
        }

        stage('Push to Hub') {
            steps {
                echo "Logging into Docker Hub..."

                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-credentials',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {

                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push '"${IMAGE_NAME}:${IMAGE_TAG}"'
                        docker logout
                    '''
                }
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully!!"
        }

        failure {
            echo "Pipeline failed!!"
        }

        always {
            echo "Cleaning workspace..."
            cleanWs()
        }
    }
}