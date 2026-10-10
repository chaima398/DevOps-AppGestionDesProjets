pipeline {
    agent any
    environment {
        DOCKERHUB_USER = 'chaima398'
        TAG = "${env.BUILD_NUMBER}"
    }
    stages {
        stage('Checkout') {
            steps { checkout scm }
        }
        stage('Build images') {
            steps { sh 'docker compose build' }
        }
        stage('Push Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds',
                                                  usernameVariable: 'HUB_USER',
                                                  passwordVariable: 'HUB_PASS')]) {
                    sh 'echo "$HUB_PASS" | docker login -u "$HUB_USER" --password-stdin'
                    sh 'docker compose push backend frontend'
                    sh 'docker logout'
                }
            }
        }
        stage('Deploy') {
            steps { sh 'docker compose up -d' }
        }
    }
}
