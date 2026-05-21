pipeline {

    agent any

    environment {
        IMAGE_NAME = "sampleapp"
        CONTAINER_NAME = "samplecontainer"
    }

    stages {

        stage('Clone Code') {
            steps {
                git branch: 'main',
                url: 'https://github.com/USERNAME/REPO.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Remove Old Container') {
            steps {
                sh '''
                docker stop $CONTAINER_NAME || true
                docker rm $CONTAINER_NAME || true
                '''
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                docker run -d \
                --name $CONTAINER_NAME \
                -p 5000:5000 \
                $IMAGE_NAME
                '''
            }
        }

        stage('Check Container') {
            steps {
                sh 'docker ps -a'
            }
        }
    }

    post {

        success {
            emailext (
                subject: "SUCCESS: Build ${BUILD_NUMBER}",
                body: """
                Build Success

                Job Name: ${JOB_NAME}

                Console Logs:
                ${BUILD_URL}console
                """,
                to: "yourmail@gmail.com"
            )
        }

        failure {
            emailext (
                subject: "FAILED: Build ${BUILD_NUMBER}",
                body: """
                Build Failed

                Check Logs:
                ${BUILD_URL}console
                """,
                to: "yourmail@gmail.com"
            )
        }
    }
}ō

