pipeline {
agent any


environment {
    IMAGE_NAME = "affaana/nodejs-demo"
}

stages {

    stage('Install Dependencies') {
        steps {
            script {
                docker.image('node:24').inside {
                    sh 'npm install'
                }
            }
        }
    }

    stage('Run Tests') {
        steps {
            script {
                docker.image('node:24').inside {
                    sh 'npm test'
                }
            }
        }
    }

    stage('Docker Login') {
        steps {
            withCredentials([
                usernamePassword(
                    credentialsId: 'dockerhub-cred',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )
            ]) {
                sh '''
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                '''
            }
        }
    }

    stage('Build Docker Image') {
        steps {
            sh "docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} ."
            sh "docker tag ${IMAGE_NAME}:${BUILD_NUMBER} ${IMAGE_NAME}:latest"
        }
    }

    stage('Push Docker Images') {
        steps {
            sh "docker push ${IMAGE_NAME}:${BUILD_NUMBER}"
            sh "docker push ${IMAGE_NAME}:latest"
        }
    }
}


}


