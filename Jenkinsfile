pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
 		sh 'echo Iniciando build...'
                sh './gradlew build'
            }
        }
    }
}
