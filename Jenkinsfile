pipeline {
    agent any

    environment {
        IMAGE_NAME = 'myapp'
        IMAGE_TAG = "${IMAGE_NAME}:${BUILD_NUMBER}"
    }

    stages {
        stage('Clone Repo') {
            steps {
                git url: 'https://github.com/yourusername/your-repo.git', branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'eval $(minikube docker-env)'  // Make sure image is built inside Minikube's Docker
                sh 'docker build -t $IMAGE_TAG .'
            }
        }

        stage('Deploy to Minikube') {
            steps {
                sh '''
                    kubectl apply -f k8s/deployment.yaml
                    kubectl rollout status deployment/myapp-deployment
                    kubectl get pods
                '''
            }
        }
    }
}
