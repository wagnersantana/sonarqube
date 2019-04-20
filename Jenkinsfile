pipeline {
    agent any

    environment {
        PUBLISH = "localbinary"
    }

    stages {
        stage('Unit Tests') {
            steps {
                sh 'echo Running unit tests...'
                sh 'mvn clean org.jacoco:jacoco-maven-plugin:prepare-agent install -Dmaven.test.failure.ignore=true -f ./server/sonar-server/src/test/projects/pom.xml'
                sh 'echo mvn sonar:sonar -f ./server/sonar-server/src/test/projects/pom.xml ------- not working'
            }
        }
        stage('Build') {
            steps {
 		sh 'echo Starting build...'
                sh './gradlew build'
            }
        }
        stage('Test') {
            steps {
                sh 'echo Starting tests...'
                sh './gradlew check'
            }
        }
    }
    post {
        always {
            echo 'End of pipeline...'
        }
        success {
            echo 'Pipeline finished SUCCESSFULLY!'
        }
        failure {
            echo 'Pipeline execution ERROR!'
        }
    }
}
