pipeline {
    agent any

    stages {

        stage('Build Docker Image') {
            steps {
                sh 'docker build --no-cache -t docker-jenkins-cicd:latest .'
            }
        }

        stage('Security Scan') {
            steps {
                sh 'trivy image --severity HIGH,CRITICAL --ignore-unfixed --exit-code 1 docker-jenkins-cicd:latest'
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
