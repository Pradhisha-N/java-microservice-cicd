pipeline {
    agent any
    environment {
        IMAGE = "pradhisha/java-microservice:${env.BRANCH_NAME}"
    }
    stages {
        stage('Clone Repo') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
        stage('Test') {
            steps {
                echo "Skipping tests for now"
            }
        }
        stage('Docker Build & Push') {
            steps {
                script {
                    docker.withRegistry('', 'dockerhub-credentials-id') {
                        sh 'docker build -t $IMAGE .'
                        sh 'docker push $IMAGE'
                    }
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
            }
        }
    }
}
