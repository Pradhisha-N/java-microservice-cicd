pipeline {
    agent any

    environment {
        IMAGE = "pradhisha/java-microservice:${env.BRANCH_NAME}"
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                echo "Skipping tests for now"
            }
        }

        stage('Docker Build & Push') {
            when {
                branch 'develop'
            }
            steps {
                script {
                    docker.withRegistry('', 'dockerhub-credentials') {
                        sh 'docker build -t $IMAGE .'
                        sh 'docker push $IMAGE'
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            when {
                branch 'develop'
            }
            steps {
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
            }
        }
    }
}
