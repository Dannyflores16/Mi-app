pipeline {
    agent any

    stages {

        stage('Prepare') {
            steps {
                echo 'Preparando el entorno...'
                bat 'git --version'
                bat 'docker --version'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Instalando dependencias...'

                // Si Node.js está instalado en Jenkins,
                // descomenta la siguiente línea:
                 bat 'npm install'

                echo 'Dependencias listas.'
            }
        }

        stage('Test') {
            steps {
                echo 'Ejecutando pruebas...'

                // Si tu proyecto tiene pruebas,
                // descomenta la siguiente línea:
                bat 'npm test'

                echo 'Pruebas finalizadas.'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Construyendo imagen Docker...'
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

        stage('Verify Published Image') {
            steps {
                echo 'Verificando imágenes publicadas...'
                bat 'docker images'
            }
        }
    }

    post {

        success {
            echo '======================================'
            echo 'Pipeline ejecutado correctamente.'
            echo 'Imagen publicada en Docker Hub.'
            echo '======================================'
        }

        failure {
            echo '======================================'
            echo 'El Pipeline falló.'
            echo 'Revisa los logs de Jenkins.'
            echo '======================================'
        }

        always {
            echo 'Pipeline finalizado.'
        }
    }
}