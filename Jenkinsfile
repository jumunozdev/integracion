pipeline {
    agent any

    environment {
        REGISTRY = "jmmunozpu"  
        APP_NAME = "prueba"
    }

    stages {
        stage('Clonar Repositorio') {
            steps {
                 git branch: 'main', url: 'https://github.com/jumunozdev/integracion.git'
            }
        }

        stage('Construir Backend') {
            steps {
                bat 'docker build -t $REGISTRY/prueba-backend:latest -f backend/Dockerfile backend'
            }
        }

        stage('Construir Frontend') {
            steps {
                bat 'docker build -t $REGISTRY/prueba-frontend:latest -f frontend/Dockerfile frontend'
            }
        }

        stage('Subir a Registro') {
            steps {
                withDockerRegistry([credentialsId: 'docker-hub-credenciales', url: '']) {
                    bat 'docker push $REGISTRY/prueba-backend:latest'
                    bat 'docker push $REGISTRY/prueba-frontend:latest'
                }
            }
        }

        stage('Desplegar en Kubernetes') {
            steps {
                bat 'kubectl apply -f k8s/'
            }
        }
    }
}
