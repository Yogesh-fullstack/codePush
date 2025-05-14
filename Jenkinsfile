pipeline {
  agent any

  environment {
    IMAGE_NAME = "ydocker12/codepush"
  }

  stages {
    stage('Build JAR') {
      steps {
        sh './gradlew build -x test'
      }
    }

    stage('Docker Build & Push') {
      steps {
        script {
          docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials-id') {
            def image = docker.build("${IMAGE_NAME}")
            image.push("latest")
          }
        }
      }
    }

    stage('Deploy') {
      steps {
        sh 'docker rm -f codepush-app || true'
        sh 'docker run -d -p 8080:8080 --name codepush-app ydocker12/codepush:latest'
      }
    }
  }
}
