pipeline {
    agent any

    environment {
        REGISTRY = "localhost:8083"
        IMAGE = "devops-app"
        TAG = "1.0"
    }

    stages {
        stage('Checkout Code') {
            steps { checkout scm }
        }

        stage('Maven Build') {
            steps { sh 'mvn clean package -DskipTests' }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t $IMAGE:$TAG .'
            }
        }

        stage('Docker Tag & Push to Nexus') {
            steps {
                sh '''
                  docker tag $IMAGE:$TAG $REGISTRY/$IMAGE:$TAG
                  docker push $REGISTRY/$IMAGE:$TAG
                '''
            }
        }
    }

    post {
        success {
            echo "Docker image successfully pushed to Nexus!"
        }
    }
}
