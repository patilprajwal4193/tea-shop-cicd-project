pipeline {
    agent any

    environment {
        IMAGE = "patilprajwal09/tea-shop-cicd-project:v1"
    }

    stages {

        stage('Clone') {
            steps {
                git(
                    branch: 'main',
                    url: 'https://github.com/patilprajwal4193/tea-shop-cicd-project.git'
                )
                 
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE .'
            }
        }

        stage('Push Docker Image') {
            steps {

                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {

                    sh 'echo $PASS | docker login -u $USER --password-stdin'

                    sh 'docker push $IMAGE'
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