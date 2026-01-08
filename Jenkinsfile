pipeline {
    agent any

    tools {
        maven 'maven'
    }

    stages {

        stage('Build Maven') {
            steps {
                git 'https://github.com/ankit-80/Devops-CI-CD--Project-2-.git'
                sh 'mvn clean install'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ankit787/devops-integration:latest .'
            }
        }

        stage('Push Image to DockerHub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-pwd',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                      echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                      docker push ankit787/devops-integration:latest
                    '''
                }
            }
        }

        stage('EKS and Kubectl Configuration') {
            steps {
                sh 'aws eks update-kubeconfig --region ap-south-1 --name ankit-cluster'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f deploymentservice.yaml'
            }
        }
    }
}
