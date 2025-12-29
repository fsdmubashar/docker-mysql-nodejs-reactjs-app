pipeline {   
    agent { label 'agent-1' }

    environment {
        imageName  = "frontend"
        imageName1 = "backend"
        dockerCred = "docker-cred"
    }

    stages {

        stage('Git checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/fsdmubashar/docker-mysql-nodejs-reactjs-app.git'
            }
        }

        stage('Build') {
            steps {
                sh "docker build -t ${imageName}:latest ./frontend"
                sh "docker build -t ${imageName1}:latest ./backend"
            }
        }

        stage('SonarQube Analysis') {
            steps {

                // 1️⃣ SonarQube credentials use ho rahi hain
                withCredentials([string(
                    credentialsId: 'Sonar-token',
                    variable: 'SONAR_TOKEN'
                )]) {

                    // 2️⃣ Jenkins tool se sonar-scanner uthaya ja raha hai
                    def scannerHome = tool 'sonar-scanner'

                    // 3️⃣ Actual SonarQube scan command
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

        stage('Trivy Scan') {
            steps {
                sh "trivy image --severity HIGH,CRITICAL --exit-code 1 ${imageName}:latest"
                sh "trivy image --severity HIGH,CRITICAL --exit-code 1 ${imageName1}:latest"
            }
        }

        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "${dockerCred}",
                    usernameVariable: "dockerHubUser",
                    passwordVariable: "dockerHubPass"
                )]) {
                    sh "docker login -u ${dockerHubUser} -p ${dockerHubPass}"
                    sh "docker tag ${imageName}:latest ${dockerHubUser}/${imageName}:latest"
                    sh "docker tag ${imageName1}:latest ${dockerHubUser}/${imageName1}:latest"
                    sh "docker push ${dockerHubUser}/${imageName}:latest"
                    sh "docker push ${dockerHubUser}/${imageName1}:latest"
                }
            }
        }

        stage('Deploy') {
            steps {
                sh "docker-compose down"
                sh "docker-compose up -d"
            }
        }
    }
}
