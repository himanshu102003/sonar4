pipeline {
    agent any
    environment {
        PATH = 'C:\\Windows\\System32'
        PYTHON_PATH = 'C:\\Users\\himan\\AppData\\Local\\Programs\\Python\\Python313;C:\\Users\\himan\\AppData\\Local\\Programs\\Python\\Python313\\Scripts'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                // Set the PATH and install dependencies using pip
                bat '''
                PATH=%PYTHON_PATH%;%PATH%
                pip install -r requirements.txt
                '''
            }
        }
        stage('SonarQube Analysis') {
            environment {
                SONAR_TOKEN = credentials('Sonarqube_token') // Accessing the SonarQube token stored in Jenkins credentials
            }
            steps {
                bat '''
                PATH=%PYTHON_PATH%;%PATH%
                sonar-scanner -Dsonar.projectKey=sonartest4 ^
                              -Dsonar.sources=. ^
                              -Dsonar.host.url=http://localhost:9000 ^
                              -Dsonar.token=%SONAR_TOKEN%
                '''
            }
        }
    }
    post {
        success {
            echo 'Pipeline completed successfully'
        }
        failure {
            echo 'Pipeline failed'
        }
        always {
            echo 'This runs regardless of the result.'
        }
    }
}
