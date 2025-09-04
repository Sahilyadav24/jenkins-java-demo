pipeline {
    agent any
    tools {
        maven 'Maven3'
    }
    environment {
        WAR_NAME = "sample.war"
        TOMCAT_DEPLOY_DIR = "C:\\tomcat9\\webapps"
        TOMCAT_PORT = "8081"  // ← Using port 8081 for Tomcat
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build with Maven') {
            steps {
                bat 'mvn clean package'  // Use 'bat' for Windows instead of 'sh'
            }
        }
        stage('Archive WAR') {
            steps {
                archiveArtifacts artifacts: "target/${WAR_NAME}", fingerprint: true
            }
        }
        stage('Deploy to Tomcat') {
            steps {
                bat "copy target\\${WAR_NAME} \"${TOMCAT_DEPLOY_DIR}\""
            }
        }
        stage('Smoke Test') {
            steps {
                bat 'timeout /t 10 /nobreak'  // Wait 10 seconds
                bat 'curl http://localhost:8081/sample/hello'  // ← Using port 8081
            }
        }
    }
    post {
        success {
            echo "Deployment successful: http://localhost:8081/sample/hello"  // ← Using port 8081
        }
    }
}