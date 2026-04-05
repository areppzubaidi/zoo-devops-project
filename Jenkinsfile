pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'zoo-website'
        CONTAINER_NAME = 'zoo-app'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/areppzubaidi/zoo-devops-project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t ${DOCKER_IMAGE}:latest .'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    sh '''
                        docker stop ${CONTAINER_NAME} || true
                        docker rm ${CONTAINER_NAME} || true
                        docker run -d --name ${CONTAINER_NAME} -p 3000:3000 ${DOCKER_IMAGE}:latest
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment successful! Visit http://${env.EC2_PUBLIC_IP}'
        }
        failure {
            echo 'Pipeline failed. Check logs.'
        }
    }
}
