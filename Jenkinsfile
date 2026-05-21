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
                url: 'https://github.com/rajubhai-hub/sample-app.git'
                credentialsId: 'Git_hub'
            }
        }


        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Stop Old Container') {
            steps {
                sh '''
                docker stop $CONTAINER_NAME || true
                docker rm $CONTAINER_NAME || true
                '''
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                docker run -d \
                --name $CONTAINER_NAME \
                -p 5000:5000 \
                $IMAGE_NAME
                '''
            }
        }

        stage('Docker Status') {
            steps {
                sh 'docker ps -a'
            }
        }
    }

    post {

        success {
            emailext (
                subject: "SUCCESS: Jenkins Build ${BUILD_NUMBER}",
                body: """
                Build Success

                Job Name: ${JOB_NAME}
                Build Number: ${BUILD_NUMBER}

                Console Output:
                ${BUILD_URL}console
                """,
                to: "yourmail@gmail.com"
            )
        }

        failure {
            emailext (
                subject: "FAILED: Jenkins Build ${BUILD_NUMBER}",
                body: """
                Build Failed

                Job Name: ${JOB_NAME}
                Build Number: ${BUILD_NUMBER}

                Check Logs:
                ${BUILD_URL}console
                """,
                to: "yourmail@gmail.com"
            )
        }
    }
}
