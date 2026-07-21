pipeline {
    agent any
    environment {
        // These IDs map to the secrets we will create in Jenkins UI
        DOCKER_CREDS = 'dockerhub-creds'
        KUBE_CREDS = 'kubeconfig-creds'
        IMAGE_NAME = 'vishalkayande/webapp' 
        TAG = "${env.BUILD_NUMBER}"
    }
    stages {
               stage('Clone Git Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/vishalkayande/Kubernetes-Jenkins-Pipeline.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:${TAG} ."
                sh "docker tag ${IMAGE_NAME}:${TAG} ${IMAGE_NAME}:latest"
            }
        }
        stage('Push to Docker Hub') {
            steps {
                // Securely injects credentials without exposing them in logs
                withCredentials([usernamePassword(credentialsId: env.DOCKER_CREDS, usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh "echo \$DOCKER_PASS | docker login -u \$DOCKER_USER --password-stdin"
                    sh "docker push ${IMAGE_NAME}:${TAG}"
                    sh "docker push ${IMAGE_NAME}:latest"
                }
            }
        }
        stage('Deploy to K8s Cluster') {
            steps {
                // Securely uses the uploaded kubeconfig file
                withKubeConfig([credentialsId: env.KUBE_CREDS]) {
                    sh "sed -i 's|${IMAGE_NAME}:latest|${IMAGE_NAME}:${TAG}|g' k8s/deployment.yaml"
                    sh "kubectl apply -f k8s/deployment.yaml"
                    sh "kubectl apply -f k8s/service.yaml"
                }
            }
        }
    }
}
