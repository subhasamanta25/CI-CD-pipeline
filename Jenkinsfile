pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Test') {
            steps {
                sh '''
                python3 -m venv venv
                . venv/bin/activate
                pip install -r requirements.txt
                pytest
                '''
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t subha250504/flask-app:latest .'
            }
        }

        stage('Push') {
            steps {
                sh 'docker push subha250504/flask-app:latest'
            }
        }
    }
}