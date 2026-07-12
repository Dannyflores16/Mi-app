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
                sh 'docker build -t mi-app .'
            }
        }

        stage('Verificar imagen') {
            steps {
                echo 'Mostrando imágenes Docker...'
                sh 'docker images'
            }
        }

    }
}