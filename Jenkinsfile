pipeline {
    agent any

    environment {
        IMAGE_NAME = "ydocker12/codepush"
    }

    stages {
        stage('Build JAR') {
            steps {
                bat './gradlew.bat build -x test'
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
                bat 'docker rm -f codepush-app || exit 0'
                bat 'docker run -d -p 8080:8080 --name codepush-app ydocker12/codepush:latest'
            }
        }
    }
}

