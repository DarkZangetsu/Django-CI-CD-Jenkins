pipeline {
    agent any

    environment {
        IMAGE_NAME = "django-devops-app"
        IMAGE_TAG = "latest"
    }

    stages {
        stage('checkout') {
            steps {
                git url: "https://github.com/DarkZangetsu/Django-CI-CD-Jenkins.git"
            }
        }

        stage('Install dependencies') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'python manage.py test'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
            }
        }

        stage('Stop and Remove Existing Container') {
            steps {
                script {
                    sh 'docker rm -f django-devops-container || true'
                }
            }
        }

        stage('Run Docker Container'){
            steps {
                sh "docker run -d --name django-devops-container -p 8000:8000 ${IMAGE_NAME}:${IMAGE_TAG}"
            }
        }
    }

    // Gestion d'erreurs
    post {
        always {
            echo "Pipeline terminé"
        }

        failure {
            echo "La pipeline a échoué"
        }
    }
    
}