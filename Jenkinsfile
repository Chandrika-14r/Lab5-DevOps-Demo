
pipeline {
    agent any

    stages {
        stage('Checkout Source') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t lab5-devops-demo:1.0 .'
            }
        }

        stage('Verify Docker Image') {
            steps {
                bat 'docker image inspect lab5-devops-demo:1.0'
            }
        }

        stage('Deploy Application') {
            steps {
                bat 'docker rm -f lab5-web 2>NUL || exit /b 0'
                bat 'docker run -d --name lab5-web -p 8082:80 lab5-devops-demo:1.0'
            }
        }

        stage('Test Application') {
            steps {
                bat 'curl.exe --fail --silent --show-error http://localhost:8082'
            }
        }
    }

    post {
        success {
            echo 'SUCCESS: Application built, deployed and tested!'
        }
        failure {
            echo 'FAILED: Check the console output for errors.'
        }
    }
}
