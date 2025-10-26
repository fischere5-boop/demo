pipeline {
    agent any

    tools {
        // Use Maven from Jenkins or download if not installed
        maven 'Maven 3'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Cloning GitHub repository...'
                git branch: 'main', url: 'https://github.com/fischere5-boop/demo.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the application...'
                sh './mvnw clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                echo 'Running unit tests...'
                sh './mvnw test'
            }
        }

        stage('Archive Artifact') {
            steps {
                echo 'Archiving the generated JAR file...'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'Build completed successfully!'
        }
        failure {
            echo 'Build failed. Check logs for details.'
        }
    }
}
