pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds')
        DOCKERHUB_USERNAME = 'chaitanya9420'
    }

    stages {
        stage('Checkout Code from GitHub') {
            steps {
                git branch: 'main', url: 'https://github.com/ChaitanyaKulkarni9400/DevOps.git'
            }
        }

        stage('Build Backend Image') {
            steps {
                script {
                    backendImage = docker.build("${DOCKERHUB_USERNAME}/backend:latest", "./backend")
                }
            }
        }

        stage('Build Frontend Image') {
            steps {
                script {
                    frontendImage = docker.build("${DOCKERHUB_USERNAME}/frontend:latest", "./frontend")
                }
            }
        }

        stage('Login & Push Images to DockerHub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-creds') {
                        backendImage.push()
                        frontendImage.push()
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                    export KUBECONFIG=/var/lib/jenkins/.kube/config
                    kubectl apply -f k8s/backend.yaml
                    kubectl apply -f k8s/frontend.yaml
                    kubectl apply -f k8s/service.yaml
                '''
            }
        }
    }
}
