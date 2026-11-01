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

        stage('Run Test Container') {
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

        stage('Deploy') {
            steps {
                sh 'docker rm -f flask-prod || true'
                sh 'docker run -d --name flask-prod -p 5002:5000 my-first-flask-app'
                echo 'Application deployed successfully.'
            }
        }
    }

    post {
        always {
            sh 'docker rm -f flask-test || true'
            echo 'Temporary test container cleaned up.'
        }

        success {
            echo 'Pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed. Check the console logs.'
        }
    }
}