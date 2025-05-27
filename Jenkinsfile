pipeline {
    agent any

    environment {
        IMAGE_NAME = "django-devops-app"
        IMAGE_TAG = "latest"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/DarkZangetsu/Django-CI-CD-Jenkins.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
            }
        }

        stage('Run Tests in Docker') {
            steps {
                sh "docker run --rm ${IMAGE_NAME}:${IMAGE_TAG} python manage.py test"
            }
        }

        stage('Stop and Remove Existing Container') {
            steps {
                script {
                    sh 'docker rm -f django-devops-container || true'
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                sh "docker run -d --name django-devops-container -p 8000:8000 ${IMAGE_NAME}:${IMAGE_TAG}"
            }
        }
    }

    post {
        always {
            echo "Pipeline terminé"
        }
        failure {
            echo "La pipeline a échoué"
        }
        success {
            echo "Pipeline exécuté avec succès!"
            echo "Application accessible sur http://localhost:8000"
        }
    }
}