pipeline {
    agent any

    stages {


        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test'
	    }
        }
	
	stage('Docker Login'){
		steps {
			withCredentials([
				usernamePassword(
					credentialsId: 'dockerhub-cred',
					usernameVariable: "DOCKER_USER",
					passwordVariable: "DOCKER_PASS"
				)
			]){
				sh '''
					echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
					'''
			}
		}
	}
        stage('Push Docker Image') {
            steps {
                sh "docker build -t affaana/nodejs-demo:${BUILD_NUMBER} ."
		sh "docker tag affaana/nodejs-demo:${BUILD_NUMBER} affaana/nodejs-demo:latest"
            }
        }
	stage('Push Build Number') {
		steps {
			sh "docker push affaana/nodejs-demo:${BUILD_NUMBER}"
			sh "docker push affaana/nodejs-demo:latest"
		}
	}
    }
}
