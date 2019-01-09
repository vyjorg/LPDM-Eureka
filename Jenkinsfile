pipeline {
    agent any
    tools {
        maven 'Apache Maven 3.5.2'
    }
    stages{
        stage('Checkout') {
            steps {
                git 'https://github.com/vyjorg/LPDM-Eureka'
            }
        }
        stage('Tests') {
            steps {
                sh 'mvn clean test'
            }
            post {
                always {
                    junit 'target/surefire-reports/**/*.xml'
                }
                failure {
                    error 'The tests failed'
                }
            }
        }
        stage('Push to DockerHub') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker stop LPDM-EurekaMS || true && docker rm LPDM-EurekaMS || true'
                sh 'docker pull vyjorg/lpdm-eureka:latest'
                sh 'docker run -d --name LPDM-EurekakMS -p 28091:28091 --restart always --memory-swappiness=0  vyjorg/lpdm-eureka:latest'
            }
        }
    }
}