cat > Jenkinsfile << 'EOF'
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'docker compose build'
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker compose down || true'
                sh 'docker compose up -d'
            }
        }

        stage('Verification') {
            steps {
                sh 'docker compose ps'
            }
        }
    }
}
EOF
