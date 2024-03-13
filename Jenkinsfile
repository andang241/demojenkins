pipeline {
    environment {
        DOCKERHUB_CREDENTIALS = credentials('DOCKERHUB')
    }
    agent any
    stages {
        stage('Build') {
            steps {
                // Clones your GitHub repository
                sh 'docker build -t andang241/test:latest .'
            }
        }
        stage('Login') {
            steps {
               sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('Push DVWA Docker Image') {
            steps {
                sh 'docker push andang241/test:latest'
            }
        }
    }
    post {
        always {
            // Clean up Docker images to avoid filling up Jenkins server
            sh "docker logout"
        }
    }
}
