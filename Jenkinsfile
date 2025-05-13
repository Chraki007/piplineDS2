pipeline {
    agent any

    environment {
        SONARQUBE = 'sonarqube' // Ensure this matches your configured SonarQube instance in Jenkins
        IMAGE_NAME = 'springboot-app' // Docker image name
        APP_PORT = '8082' // Port for the application
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Chraki007/piplineDS2.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Analyze SonarQube') {
            steps {
                withSonarQubeEnv("${SONARQUBE}") {
                    sh 'mvn sonar:sonar -Dsonar.projectKey=springboot-project -Dsonar.host.url=http://localhost:9000'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }

        stage('Run Docker Container') {
            steps {
                sh "docker run -d -p ${APP_PORT}:8080 ${IMAGE_NAME}"
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline executed successfully!"
        }
        failure {
            echo "❌ Pipeline failed!"
        }
    }
}
