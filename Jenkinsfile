pipeline {
    agent { label 'agent-1' }

    environment {
        FRONTEND_IMAGE = "frontend-app"
        BACKEND_IMAGE  = "backend-app"
        dockerCred = "docker-cred"
    }

    stages {

        stage('Git Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/fsdmubashar/docker-mysql-nodejs-reactjs-app.git'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withCredentials([string(
                    credentialsId: 'Sonar-token',
                    variable: 'SONAR_TOKEN'
                )]) {
                    script {
                        def scannerHome = tool 'sonar-scanner'

                        sh """
                        ${scannerHome}/bin/sonar-scanner \
                        -Dsonar.projectKey=nodejs-docker-app \
                        -Dsonar.sources=frontend,backend \
                        -Dsonar.host.url=http://SONARQUBE_IP:9000 \
                        -Dsonar.login=${SONAR_TOKEN}
                        """
                    }
                }
            }
        }

        stage('Build Docker Images') {
            steps {
                sh 'docker build -t frontend-app ./frontend'
                sh 'docker build -t backend-app ./backend'
            }
        }

        stage('Trivy Image Scan') {
            steps {
                sh 'trivy image frontend-app'
                sh 'trivy image backend-app'
            }
        }

        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: ${dockerCred},
                    usernameVariable: 'DOCKERHUB_USER',
                    passwordVariable: 'DOCKERHUB_PASS'
                )]) {

                    sh 'docker login -u $DOCKERHUB_USER -p $DOCKERHUB_PASS'

                    sh 'docker tag frontend-app $DOCKERHUB_USER/frontend-app:latest'
                    sh 'docker tag backend-app  $DOCKERHUB_USER/backend-app:latest'

                    sh 'docker push $DOCKERHUB_USER/frontend-app:latest'
                    sh 'docker push $DOCKERHUB_USER/backend-app:latest'
                }
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker-compose down'
                sh 'docker-compose up -d'
            }
        }
    }

    post {
        success {
            echo 'CI/CD Pipeline Successfully Completed'
        }
        failure {
            echo 'Pipeline Failed – Please Check Logs'
        }
    }
}
