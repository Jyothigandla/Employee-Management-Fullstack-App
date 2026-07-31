pipeline {
    agent any

    environment {
        APP_DIR = "/home/ubuntu/Employee-Management-Fullstack-App"
    }

    stages {

        stage('Checkout') {
            steps {
                git(
                    branch: 'master',
                    credentialsId: 'github-creds',
                    url: 'https://github.com/Jyothigandla/Employee-Management-Fullstack-App.git'
                )
            }
        }

        stage('Build Backend') {
            steps {
                sh """
                    cd ${APP_DIR}/backend
                    mvn clean package -DskipTests
                """
            }
        }

        stage('Build Frontend') {
            steps {
                sh """
                    cd ${APP_DIR}/frontend
                    npm install
                    npm run build
                """
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
                sh """
                    echo "========== Docker Containers =========="
                    docker ps

                    echo ""
                    echo "========== Docker Compose =========="
                    docker compose -f ${APP_DIR}/docker-compose.yml ps

                    echo ""
                    echo "========== Backend =========="
                    curl -I http://localhost:8081/swagger-ui/index.html || true

                    echo ""
                    echo "========== Frontend =========="
                    curl -I http://localhost:3000 || true
                """
            }
        }
    }

    post {

        success {
            echo "===================================="
            echo "Deployment completed successfully!"
            echo "===================================="
        }

        failure {
            echo "===================================="
            echo "Deployment failed!"
            echo "===================================="
        }

        always {
            sh """
                docker ps -a || true
            """
        }
    }
}
