pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/kingonlook-stack/docker-jenkins-cicd.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t docker-jenkins-cicd:latest .'
            }
        }

        stage('Security Scan') {
            steps {
                sh 'trivy image --severity HIGH,CRITICAL --exit-code 1 docker-jenkins-cicd:latest'
            }
        }

        stage('Stop Old Container') {
            steps {
                sh 'docker rm -f docker-jenkins-cicd-app || true'
            }
        }

        stage('Run New Container') {
            steps {
                sh 'docker run -d --name docker-jenkins-cicd-app -p 5000:5000 docker-jenkins-cicd:latest'
            }
        }

        stage('Test Application') {
            steps {
                sh 'sleep 3'
                sh 'curl -f http://localhost:5000/'
            }
        }
    }
}
