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

        stage('Login') {
            steps {
                // Login docker hub
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        
        stage('Push DVWA Docker Image') {
            steps {
                // Log in and push the DVWA Docker image to Docker Hub
                        sh 'docker push $DVWA_IMAGE'
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
                // push the MYSQL Docker image to Docker Hub
                        sh 'docker push $MYSQL_IMAGE'
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
