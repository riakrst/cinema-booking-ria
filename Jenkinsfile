pipeline {
    agent any

    environment {
        COMPOSE_PROJECT_NAME = 'cinema-booking'
    }

    stages {
        stage('Checkout') {
            steps {
                // Menggunakan konfigurasi SCM bawaan Jenkins agar dinamis dan menghindari masalah case-sensitive branch
                checkout scm
            }
        }

        stage('Build Images') {
            steps {
                echo 'Building Docker images...'
                sh 'docker compose build --no-cache'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                // Stop container lama, lalu jalankan yang baru
                sh 'docker compose down'
                sh 'docker compose up -d'
            }
        }

        stage('Cleanup') {
            steps {
                echo 'Cleaning up dangling images...'
                sh 'docker image prune -f'
            }
        }
    }

    post {
        success {
            echo 'Pipeline berhasil! Aplikasi sudah running.'
            echo 'Frontend : http://localhost:8084'
            echo 'Backend  : http://localhost:5001'
        }
        failure {
            echo 'Pipeline gagal! Cek log di atas untuk detail error.'
        }
        always {
            // Bersihkan workspace Jenkins
            cleanWs()
        }
    }
}
