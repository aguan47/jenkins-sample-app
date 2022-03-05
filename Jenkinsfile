pipeline {
	
	agent any

	stages {
		stage("build the app") {
			steps {
				echo 'building the app'
				dir('app') {
					sh 'npm install'
				}
			}
		}
		
		stage("test") {
			steps {
				echo 'testing the app'
				dir('app') {
					sh 'npm test'
				}
			}
		}

		stage("build docker image") {
			when {
				expression {
					BRANCH_NAME == 'master'
				}
			}
			steps {
				echo 'building the docker image'
				withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'PASSWORD', usernameVariable: 'USER')]) {
		sh 'docker build -t alguan47/jenkins:node-app-2.0 .'
		sh "echo $PASSWORD | docker login -u $USER --password-stdin"
		sh 'docker push alguan47/jenkins:node-app-2.0'
	}
			}
		} 
				

		stage("deploy") {
			when {
				expression {
					BRANCH_NAME == 'master'	
				}
			}
			steps {
				echo 'deploying the app'
			}
		}
	}
}
