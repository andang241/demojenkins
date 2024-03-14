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

        stage('Login to Docker Hub') {
            steps {
                script {
                    // Đăng nhập vào Docker Hub
                    docker.withRegistry('https://index.docker.io/v1/', 'DOCKERHUB') {
                        // Các bước sử dụng Docker có thể được thêm vào đây
                        echo 'Đã đăng nhập thành công vào Docker Hub'
                    }
                }
            }
        }
        stage('Build and Push DVWA Docker Image') {
        steps {
            script {
                dir('dvwa') {
                    def dvwa = docker.build(DVWA_IMAGE)
                    docker.withRegistry('https://index.docker.io/v1/', 'DOCKERHUB') {
                    dvwa.push()
                    }    
                }
            }
        }
    }
        stage('Build and Push MYSQL Docker Image') {
        steps {
            script {
                dir('mysql') {
                    def mysql = docker.build(MYSQL_IMAGE)
                    docker.withRegistry('https://index.docker.io/v1/', 'DOCKERHUB') {
                    mysql.push()
                    }    
                }
            }
        }
    }

        stage('clone repo to  sshagent') {
            steps {
                sshagent(['ssh_agent']) {
                    sh 'ssh -o StrictHostKeyChecking=no misa@10.1.36.38 "git clone https://github.com/andang241/demojenkins.git"'
                }
            }
        }

        stage('Pull Docker Images') {
            steps {
                script {
                    // Pull images từ Docker Hub
                    sh 'ssh -o StrictHostKeyChecking=no misa@10.1.36.38 "docker pull andang241/dvwa"'
                    sh 'ssh -o StrictHostKeyChecking=no misa@10.1.36.38 "docker pull andang241/mysql"'
                }
            }
        }

        // stage('Build container using docker-compose') {
        //     steps {
        //         script {
        //             // Pull images từ Docker Hub
        //             sh 'ssh -o StrictHostKeyChecking=no andang241@10.1.37.34 "docker-compose up -d"'
        //         }
        //     }
        // }

    }
    post {
        always {
            // Clean up Docker images to avoid filling up Jenkins server
            sh 'docker logout'
        }
    }
}
