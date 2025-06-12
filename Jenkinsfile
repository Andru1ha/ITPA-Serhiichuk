pipeline {
	agent any

	stages {
		stage('Start') {
			steps {
				echo 'Lab2: started by GitHub'
			}
		}

		stage('Image build') {
			steps{
				sh "docker build -t andru1ha:latest ."
				sh "docker tag andru1ha andu1ha/andru1ha:latest"
				sh "docker tag andru1ha andru1ha/andru1ha:$BUILD_NUMBER"
			}
		}

		stage('Push to registry') {
			steps {
				withDockerRegistry([ credentialId: "dockerhub-token, url: ""]) {
					sh "docker push andru1ha/andru1ha:latest"
					sh "docker push andru1ha/andru1ha:$BUILD_NUMBER"
				}
			}
		}
		
		stage('Deploy image') {
			steps{
				sh "docker run -d -p 80:80 andru1ha/andru1ha"
			}

			}
		}

		stage('Deploy nginx/custom'){
			steps{
				sh "docker run -d -p 80:80 nginx/custom:latest"
			}
		}
	}
}
