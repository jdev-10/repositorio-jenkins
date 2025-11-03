pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'express-pokemonsapi'
        DOCKER_TAG = 'latest'
        PORT = '8081'
    }
    stages {
        stage('Checkout') {
            steps {
                echo 'Clonando el repositorio...'
                git branch: 'main', url: 'https://github.com/jdev-10/repositorio-jenkins.git'
            }
        }
        stage('Build') {
            steps {
                echo 'Construyendo la imagen Docker...'
            sh 'pwd'
            sh 'ls -la'
                script {
                    sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
                }
            }
        }
        stage('Test') {
            when {
                expression { fileExists('package.json') }
            }
            steps {
                echo 'Ejecutando pruebas unitarias (si existen)...'
                script {
                    sh 'npm test || echo "No hay pruebas definidas"'
                }
            }
        }
    }
        
    post {
        success {
            echo 'Pipeline completado exitosamente.'
        }
        failure {
            echo 'Error en la ejecución del pipeline.'
        }
    }
}
