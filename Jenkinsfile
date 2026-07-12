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

                script {
    def commitSHA = bat(
        script: '@git rev-parse --short HEAD',
        returnStdout: true
    ).trim().split('\r?\n')[-1]

    withCredentials([usernamePassword(
        credentialsId: 'dockerhub-credentials',
        usernameVariable: 'DOCKER_USER',
        passwordVariable: 'DOCKER_PASS'
    )]) {

        bat """
        docker login -u %DOCKER_USER% -p %DOCKER_PASS%

        docker tag mi-app:latest %DOCKER_USER%/mi-app:latest
        docker tag mi-app:latest %DOCKER_USER%/mi-app:build-%BUILD_NUMBER%
        docker tag mi-app:latest %DOCKER_USER%/mi-app:${commitSHA}

        docker push %DOCKER_USER%/mi-app:latest
        docker push %DOCKER_USER%/mi-app:build-%BUILD_NUMBER%
        docker push %DOCKER_USER%/mi-app:${commitSHA}
        """
                    }
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