pipeline {
    agent any

    environment {
        IMAGE_NAME = "techcorp-flask"
        CONTAINER_NAME = "techcorp-staging"
        APP_PORT = "8081"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Test simple') {
            steps {
                sh 'python3 -m py_compile app.py'
            }
        }

        stage('Build Docker') {
            steps {
                sh 'docker build -t ${IMAGE_NAME}:latest .'
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker rm -f ${CONTAINER_NAME} || true'
                sh 'docker run -d --name ${CONTAINER_NAME} -p ${APP_PORT}:5000 ${IMAGE_NAME}:latest'
            }
        }

        stage('Verification') {
            steps {
                sh 'sleep 5'
                sh 'curl -f http://localhost:${APP_PORT}/health'
            }
        }
    }
}
