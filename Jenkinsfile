pipeline {
    
    agent any
        
    stages {
        stage('Sonarqube Analysis'){
            steps {
                sh 'mvn sonar:sonar -Dsonar.projectKey=gateway -Dsonar.host.url=https://sonar.mezi.space -Dsonar.login=4293fbc64f730c76ed633c4c123f3c6506bba536'
            }
        }
        stage('MVN Install'){
            steps {
                sh """
                mvn clean install -DskipTests
                """
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t meziaris/gateway:$BUILD_NUMBER .'
            }
        }
        stage('Push Image to DockerHub') {
            steps {
				sh 'docker push meziaris/gateway:$BUILD_NUMBER'
            }
        }
        stage('Deploy to Docker') {
            steps {
                checkout scm
                sh """
                sed -i 's/latest/$BUILD_NUMBER/g' docker-compose.yml
                docker-compose up -d
                """
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