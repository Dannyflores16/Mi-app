pipeline {
    agent any

    stages {

        stage('Clonar repositorio') {
            steps {
                echo 'Descargando código desde GitHub...'
            }
        }

        stage('Construir imagen Docker') {
            steps {
                echo 'Construyendo imagen...'
                bat 'docker build -t mi-app .'
            }
        }

        stage('Push to Registry') {
    steps {
        echo 'Subiendo imagen a Docker Hub...'

        withCredentials([usernamePassword(
            credentialsId: 'dockerhub-credentials',
            usernameVariable: 'DOCKER_USER',
            passwordVariable: 'DOCKER_PASS'
        )]) {

            bat '''
            docker login -u %DOCKER_USER% -p %DOCKER_PASS%
            docker tag mi-app:latest %DOCKER_USER%/mi-app:latest
            docker push %DOCKER_USER%/mi-app:latest
            '''
        }
    }
}

        stage('Verificar imagen') {
            steps {
                echo 'Mostrando imágenes Docker...'
                bat 'docker images'
            }
        }

    }
}