pipeline {
	  agent any
	  enviroment {
		DOCKERHUB_CREDS = credentials('docker')
	  }
	
	stages {

	stage('Docker Image Build') {
		steps {
			echo 'Building Docker Image...'
			sh ' docker build --tag ccrimson1/cw2-server:0.1 . '
			echo 'Docker Image built successfully'
		}
	}

	stage('Test Docker Image') {
		steps {
			echo 'Testing Docker Image...'
			sh ```
				docker image inspect/cw2-server:0.1
				docker run --name test-container -p 8080 -d ccrimson1/cw2-server:0.1
				docker ps
				docker stop test-container
				docker rm test-container
			```
		}
	
	}
	
	stage('Dockerhub Login') {
		steps {
			sh 'ehco $DOCKERHUB_CREDS_PSW | docker login -u $DOCKERHUB_CREDS_USR --password-stdin'
		}
	}

	stage('DockerHub Image Push') {
		steps {
			sh 'docker push ccrimson/cw2-server:0.1'
		}
	}

	stage('Deploy') {
		steps {
			ssh(['jenkins-k8s-ssh-key'
