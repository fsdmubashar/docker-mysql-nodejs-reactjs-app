pipeline {   
    agent { label 'agent-1' }

    environment {
        imageName = "frontend"
        imageName1 = "backend"
        dockerCred = "docker-cred"  // credentials ID
    }

    stages {

        stage('Git checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/fsdmubashar/docker-mysql-nodejs-reactjs-app.git'
            }
        }
        
        stage('Build') {
            steps {
                sh "docker build -t ${imageName} ./frontend"
                sh "docker build -t ${imageName1} ./backend"
            }
        }
        
        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "${dockerCred}",
                    passwordVariable: "dockerHubPass",
                    usernameVariable: "dockerHubUser"
                )]){
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker image tag ${imageName} ${env.dockerHubUser}/${imageName}:latest"
                    sh "docker image tag ${imageName1} ${env.dockerHubUser}/${imageName1}:latest"
                    sh "docker push ${env.dockerHubUser}/${imageName}:latest"
                    sh "docker push ${env.dockerHubUser}/${imageName1}:latest"
                }
            }
        }
        
        stage('Deploy') {
            steps {
                sh 'docker-compose down && docker-compose up -d'
            }
        }
        
    }
}
