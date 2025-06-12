pipeline {
  agent any

  stages {
    stage('Start') {
      steps {
        echo 'Lab2: started by GitHub'
      }
    }

    stage('Image build') {
      steps {
        sh "docker build -t andru1ha/itpa-serhiichuk:latest ."
        sh "docker tag andru1ha/itpa-serhiichuk:latest andru1ha/itpa-serhiichuk:${BUILD_NUMBER}"
      }
    }

    stage('Push to registry') {
      steps {
        withDockerRegistry([ credentialsId: 'dockerhub-creds', url: '' ]) {
          sh "docker push andru1ha/itpa-serhiichuk:latest"
          sh "docker push andru1ha/itpa-serhiichuk:${BUILD_NUMBER}"
        }
      }
    }

    stage('Deploy image') {
      steps {
        sh '''
          CONTAINER_ID=$(docker ps -q --filter ancestor=andru1ha/itpa-serhiichuk:latest)
          if [ ! -z "$CONTAINER_ID" ]; then
            docker rm -f $CONTAINER_ID
          fi
          docker run -d -p 80:80 andru1ha/itpa-serhiichuk:latest
        '''
      }
    }

    stage('Deploy nginx/custom') {
      steps {
        sh '''
          CONTAINER_ID=$(docker ps -q --filter ancestor=nginx/custom:latest)
          if [ ! -z "$CONTAINER_ID" ]; then
            docker rm -f $CONTAINER_ID
          fi
          docker run -d -p 8081:80 nginx/custom:latest
        '''
      }
    }
  }
}
