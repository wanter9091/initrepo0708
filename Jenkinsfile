pipeline {
    agent any

    environment {
        FRONT_IMAGE = "react-app"
        FRONT_CONTAINER = "react-app-container"
        FRONT_PORT = "80"
        BACK_IMAGE = "fastapi-app"
        BACK_CONTAINER = "fastapi-app-container"
        BACK_PORT = "8080"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Frontend Docker Image') {
            steps {
                dir('frontend') {
                    sh "docker build -t $FRONT_IMAGE ."
                }
            }
        }

        stage('Build Backend Docker Image') {
            steps {
                dir('backend') {
                    sh "docker build -t $BACK_IMAGE ."
                }
            }
        }

        stage('Deploy Containers') {
            steps {
                sh "docker rm -f $FRONT_CONTAINER || true"
                sh "docker rm -f $BACK_CONTAINER || true"

                sh "docker run -d -p ${FRONT_PORT}:80 --name $FRONT_CONTAINER $FRONT_IMAGE"
                sh "docker run -d -p ${BACK_PORT}:8080 --name $BACK_CONTAINER $BACK_IMAGE"
            }
        }

        stage('Check Running Container') {
            steps {
                echo "✅ 현재 실행 중인 컨테이너 목록:"
                sh "docker ps"
            }

        }
    }

    post {
        success {
            echo "✅ 배포 성공!"
        }
        failure {
            echo "❌ 배포 실패!"
        }
    }
}
