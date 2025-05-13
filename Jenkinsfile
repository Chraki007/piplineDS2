pipeline {
    agent any

    environment {
        SONARQUBE = 'SonarQubeServer' // Nom du serveur SonarQube configuré dans Jenkins (Manage Jenkins > Configure System)
        DOCKER_IMAGE = 'springboot-app' // Nom de l’image Docker
    }

    stages {
        stage('Cloner le projet GitHub') {
            steps {
                git branch: 'main', url: 'github.com/Chraki007/piplineDS2.git'
            }
        }

        stage('Build Maven') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Analyse SonarQube') {
            steps {
                withSonarQubeEnv("${SONARQUBE}") {
                    sh 'mvn sonar:sonar -Dsonar.projectKey=springboot-project -Dsonar.host.url=http://localhost:9000'
                }
            }
        }

        stage('Attente du Quality Gate') {
            steps {
                timeout(time: 1, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_IMAGE} ."
                }
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline terminé avec succès."
        }
        failure {
            echo "❌ Pipeline échoué."
        }
    }
}
