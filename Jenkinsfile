pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "docker push jprianon/sock-shop-carts"
        DOCKER_CREDENTIALS_ID = '1dee9432-28cc-47b9-85af-c0e556775fd0'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/jprianon/carts.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    dockerImage = docker.build("${DOCKER_IMAGE}:${env.BUILD_ID}")
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    dockerImage.inside {
                        sh 'pytest'
                    }
                }
            }
        }

        stage('Push') {
            steps {
                script {
                    docker.withRegistry('', DOCKER_CREDENTIALS_ID) {
                        dockerImage.push("${env.BUILD_ID}")
                        dockerImage.push('latest')
                    }
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
