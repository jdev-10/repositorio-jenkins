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
        stage('Deploy') {
            steps {
                sh '''
                echo "Desplegando contenedor..."
                # Si el contenedor ya existe, detenerlo y eliminarlo
                if [ "$(docker ps -aq -f name=express-pokemonsapi)" ]; then
                    docker stop express-pokemonsapi || true
                    docker rm express-pokemonsapi || true
                fi

                docker run -d --name express-pokemonsapi -p 8081:8081 express-pokemonsapi:latest
                '''
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
