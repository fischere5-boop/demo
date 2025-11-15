pipeline {
    agent any

    tools {
        maven 'Maven 3'
    }

    environment {
        NEXUS_URL = "http://localhost:8081"
        NEXUS_REPO = "hw6"
        NEXUS_CREDENTIALS = "nexus-credentials"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/fischere5-boop/demo.git'
            }
        }

        stage('Build JAR') {
            steps {
                sh "mvn clean package -DskipTests"
            }
        }

        stage('Upload to Nexus') {
            steps {
                script {
                    def jarFile = sh(script: "ls target/*.jar", returnStdout: true).trim()
                    echo "Uploading: ${jarFile}"
        
                    withCredentials([
                        usernamePassword(
                            credentialsId: env.NEXUS_CREDENTIALS,
                            usernameVariable: 'NEXUS_USER',
                            passwordVariable: 'NEXUS_PASS'
                        )
                    ]) {
                        sh """
                        mvn deploy:deploy-file \
                          -Dfile=${jarFile} \
                          -DrepositoryId=${env.NEXUS_REPO} \
                          -Durl=${env.NEXUS_URL}/repository/${env.NEXUS_REPO}/ \
                          -DgroupId=com.example.demo \
                          -DartifactId=demo \
                          -Dversion=1.0.0 \
                          -Dpackaging=jar \
                          -DgeneratePom=true \
                          -Dusername=$NEXUS_USER \
                          -Dpassword=$NEXUS_PASS
                        """
                    }
                }
            }
        }
    }
}
