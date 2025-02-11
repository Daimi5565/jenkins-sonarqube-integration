pipeline {
    agent any

    environment {
        // Use the SonarQube server name configured in Jenkins
        SONARQUBE_SERVER = 'SonarQube'
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Checkout code from the Git repository
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // Build the project using Maven
                sh 'mvn clean install'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                // Use SonarQube environment
                withSonarQubeEnv("${SONARQUBE_SERVER}") {
                    // Run SonarQube analysis
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Quality Gate') {
            steps {
                // Wait for SonarQube to complete analysis and check the quality gate
                waitForQualityGate abortPipeline: true
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution completed.'
        }

        failure {
            echo 'Pipeline failed. Check logs for details.'
        }

        success {
            echo 'Pipeline executed successfully.'
        }
    }
}
