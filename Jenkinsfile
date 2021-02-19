pipeline {
    
    agent any
        
    stages {
        stage('Sonarqube analysis'){
            steps {
                sh """
                   mvn sonar:sonar \
                   -Dsonar.projectKey=gateway \
                   -Dsonar.host.url=https://sonar.mezi.space \
                   -Dsonar.login=6cf36ca551b4fe65e2b2ba9c6847a504ae28d707
                   """
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t meziaris/gateway:$BUILD_NUMBER .'
            }
        }
        stage('Push Image to Docker hub') {
            steps {
				sh 'docker push meziaris/gateway:$BUILD_NUMBER'
            }
        }
        stage('Deploy to Docker') {
            steps {
                checkout scm
                sh 'docker-compose up -d'
            }
        }
        stage('Remove docker image last build Dev') {
            steps {
                sh 'docker rmi swadharma/gateway:$BUILD_NUMBER'
                sh 'docker rmi swadharma/gateway:latest'
            }
        }
        stage('Git') {
            steps {
                step([$class: 'WsCleanup'])
                checkout scm
            }
        }
    }
}