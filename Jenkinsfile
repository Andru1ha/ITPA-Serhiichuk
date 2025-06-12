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
        // Зупинити попередній контейнер (опціонально)
        sh "docker rm -f \$(docker ps -q --filter ancestor=andru1ha/itpa-serhiichuk:latest)  true"
        // Запуск на порту 80
        sh "docker run -d -p 80:80 andru1ha/itpa-serhiichuk:latest"
      }
    }

    stage('Deploy nginx/custom') {
      steps {
        // Зупинити попередній контейнер (опціонально)
        sh "docker rm -f \$(docker ps -q --filter ancestor=nginx/custom:latest)  true"
        // Запуск на порту 8080
        sh "docker run -d -p 8080:80 nginx/custom:latest"
      }
    }
  }
}
