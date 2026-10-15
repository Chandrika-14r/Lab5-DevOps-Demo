
pipeline {
    agent any

    environment {
        DOCKER_CLI = 'C:/Users/HP/AppData/Local/Programs/DockerDesktop/resources/bin/docker.exe'
        KUBECTL_CLI = 'C:/Users/HP/AppData/Local/Programs/DockerDesktop/resources/bin/kubectl.exe'
        KUBECONFIG = 'C:/Users/HP/.kube/config'
    }

    stages {
        stage('Checkout Source') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                bat '"%DOCKER_CLI%" build -t lab5-devops-demo:1.0 .'
            }
        }

        stage('Verify Docker Image') {
            steps {
                bat '"%DOCKER_CLI%" image inspect lab5-devops-demo:1.0'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                bat '"%KUBECTL_CLI%" get nodes'
                bat '"%KUBECTL_CLI%" apply -f k8s-deployment.yaml'
                bat '"%KUBECTL_CLI%" apply -f k8s-service.yaml'
                bat '"%KUBECTL_CLI%" rollout restart deployment/devops-demo'
                bat '"%KUBECTL_CLI%" rollout status deployment/devops-demo --timeout=120s'
                bat '"%KUBECTL_CLI%" get deployments,pods,services'
            }
        }

        stage('Test Application') {
            steps {
                bat 'curl.exe --fail --silent --show-error http://localhost:30082'
            }
        }
    }

    post {
        success {
            echo 'SUCCESS: Docker build, Kubernetes deployment and application test completed!'
        }
        failure {
            echo 'FAILED: Check Console Output for the exact error.'
        }
    }
}
