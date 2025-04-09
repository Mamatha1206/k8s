pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git (url: 'https://github.com/Mamatha1206/k8s.git',branch:'main')
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("mamatha0124/helloworld-java:v${BUILD_NUMBER}")
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'DOCKER_CREDENTIALS_ID', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    script {
                        sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                        sh "docker push mamatha0124/helloworld-java:v${BUILD_NUMBER}"
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh "kubectl set image deployment/helloworld-deployment helloworld=mamatha0124/helloworld-java:v${BUILD_NUMBER}"
                }
            }
        }
    }
}
