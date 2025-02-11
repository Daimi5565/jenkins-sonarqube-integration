pipeline {
    agent any

    environment {
        SONAR_URL = 'http://localhost:9000' // SonarQube URL
        SONAR_AUTH_TOKEN = credentials('sonarqube-token') // Replace 'sonarqube-token' with your Jenkins credential ID
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build & Test') {
            steps {
                script {
                    sh 'mvn clean test'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') { // Replace 'SonarQube' with your Jenkins SonarQube server name
                    sh 'mvn sonar:sonar -Dsonar.projectKey=jenkins-sonarqube-integration -Dsonar.host.url=${SONAR_URL} -Dsonar.login=${SONAR_AUTH_TOKEN}'
                }
            }
        }

        stage('Quality Gate') {
            steps {
                waitForQualityGate abortPipeline: true
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check the logs.'
        }
    }
}
