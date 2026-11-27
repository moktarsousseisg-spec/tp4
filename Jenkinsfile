pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build images') {
            steps {
                sh 'docker build -t mern-server ./Server'
                sh 'docker build -t mern-client ./Client'
            }
        }

        stage('Stop anciens conteneurs') {
            steps {
                sh 'docker compose down || true'
            }
        }

        stage('Deploy avec Docker Compose') {
            steps {
                sh 'docker compose up -d --build'
            }
        }

        stage('Verification') {
            steps {
                sh 'docker compose ps'
                sh 'sleep 5'
                sh 'docker exec server printenv MONGO_URI'
            }
        }
    }

    post {
        success {
            echo 'Pipeline termine avec succes'
        }
        failure {
            echo 'Echec du pipeline'
            sh 'docker compose logs || true'
        }
    }
}
