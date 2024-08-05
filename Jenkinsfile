pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "sandhyaanpat/docker-ci:latest"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/SandhyaAnpat/CompleteCI.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    docker.build(DOCKER_IMAGE)
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    docker.image(DOCKER_IMAGE).inside {
                        sh 'npm test'
                    }
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials-id') {
                        docker.image(DOCKER_IMAGE).push()
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    docker.image(DOCKER_IMAGE).run('-d -p 8080:8080')
                }
            }
        }
    }
}

