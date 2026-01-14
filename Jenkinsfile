pipeline {
    agent {
        docker {
            image 'docker:27-cli' 
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }

    environment {
        MAVEN_IMAGE = 'maven:3.9.6-eclipse-temurin-17'
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Maven Build') {
            steps {
                sh '''
                  docker run --rm \
                  -v "$PWD":/app \
                  -w /app \
                  $MAVEN_IMAGE \
                  mvn clean package -DskipTests
                '''
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t devops-app:1.0 .'
            }
        }
    }

    post {
        success {
            echo 'CI Pipeline completed successfully!'
        }
    }
}
