pipeline {
    agent any

    environment {
        APP_DIR = "/home/ubuntu/Employee-Management-Fullstack-App"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/Jyothigandla/Employee-Management-Fullstack-App.git'
            }
        }

        stage('Build Backend') {
            steps {
                dir("${APP_DIR}/backend") {
                    sh 'mvn clean package -DskipTests'
                }
            }
        }

        stage('Build Frontend') {
            steps {
                dir("${APP_DIR}/frontend") {
                    sh 'npm install'
                    sh 'npm run build'
                }
            }
        }

        stage('Deploy') {
            steps {
                sh """
                    cd ${APP_DIR}
                    docker compose down || true
                    docker compose up -d --build
                """
            }
        }

        stage('Verify') {
            steps {
                sh '''
                    docker ps
                    docker compose ps
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Deployment completed successfully!'
        }

        failure {
            echo '❌ Deployment failed.'
        }

        always {
            sh 'docker ps -a || true'
        }
    }
}
