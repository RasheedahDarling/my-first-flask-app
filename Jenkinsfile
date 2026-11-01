pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                deleteDir()

                git branch: 'main',
                    url: 'https://github.com/RasheedahDarling/my-first-flask-app.git'
            }
        }

        stage('Inspect Files') {
            steps {
                sh 'ls -la'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t my-first-flask-app .'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker rm -f flask-test || true'
                sh 'docker run -d --name flask-test -p 5001:5000 my-first-flask-app'
            }
        }

        stage('Test App') {
            steps {
                sh 'sleep 3'
                sh 'curl -f http://host.docker.internal:5001'
            }
        }

        stage('Use Secret') {
            steps {
                withCredentials([string(credentialsId: 'demo-secret', variable: 'DEMO_TOKEN')]) {
                    sh 'echo "Jenkins received the secret safely"'
                }
            }
        }
    }
}