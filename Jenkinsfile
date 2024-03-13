pipeline {
    environment {
        DOCKERHUB_CREDENTIALS = credentials('DOCKERHUB')
        DVWA_IMAGE = 'andang241/dvwa'
        MYSQL_IMAGE = 'andang241/mysql'
    }
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                // Clones your GitHub repository
                git 'https://github.com/andang241/demojenkins.git'
            }
        }
        stage('Build DVWA Docker Image') {
            steps {
                // Changes directory to 'dvwa' and builds the Docker image
                dir('dvwa') {
                    script {
                        docker.build(DVWA_IMAGE)
                    }
                }
            }
        }
        stage('Push DVWA Docker Image') {
            steps {
                // Log in and push the DVWA Docker image to Docker Hub
                script {
                    docker.withRegistry('', DOCKERHUB_CREDENTIALS) {
                        docker.image(DVWA_IMAGE).push()
                    }
                }
            }
        }
        stage('Build MYSQL Docker Image') {
            steps {
                // Changes directory to 'mysql' and builds the Docker image
                dir('mysql') {
                    script {
                        docker.build(MYSQL_IMAGE)
                    }
                }
            }
        }
        stage('Push MYSQL Docker Image') {
            steps {
                // Log in and push the MYSQL Docker image to Docker Hub
                script {
                    docker.withRegistry('', DOCKERHUB_CREDENTIALS) {
                        docker.image(MYSQL_IMAGE).push()
                    }
                }
            }
        }
    }
    post {
        always {
            // Clean up Docker images to avoid filling up Jenkins server
            sh "docker rmi -f \$(docker images -q)"
        }
    }
}
