pipeline {
    agent any

    environment {
        AWS_DEPLOY = 'true'
        AWS_ACCESS = credentials('AWS-SAMSUNG')
        AWS_SITE  = 'ec2-54-149-127-183.us-west-2.compute.amazonaws.com'
        AWS_USER = 'ubuntu'
        AWS_PEM_FILE = '/var/jenkins_home/aws-samsung-access.pem'
    }

    stages {
        stage('Unit Tests') {
            steps {
                sh 'echo Running unit tests...'
                sh 'echo mvn clean org.jacoco:jacoco-maven-plugin:prepare-agent install -Dmaven.test.failure.ignore=true -f ./server/sonar-server/src/test/projects/pom.xml'
                sh 'echo mvn sonar:sonar -f ./server/sonar-server/src/test/projects/pom.xml ------- not working'
            }
        }
        stage('Build') {
            steps {
 		sh 'echo Starting build...'
                sh 'echo ./gradlew build'
            }
        }
        stage('Deploy AWS') {
            when {
                expression {
                    return env.AWS_DEPLOY == 'true';
                }
            }
            steps {
                sh "echo Deploying to AWS server: $AWS_SITE"
                sh "/var/jenkins_home/awspublish.sh $AWS_SITE $AWS_USER $AWS_PEM_FILE sonar-application/build/distributions/*.zip"
            }
        }
    }
    post {
        always {
            echo 'End of pipeline...'
        }
        success {
            echo 'Pipeline finished SUCCESSFULLY!'
            archiveArtifacts artifacts: 'sonar-application/build/distributions/*.zip', fingerprint: true
            deleteDir()
        }
        failure {
            echo 'Pipeline execution ERROR!'
        }
    }
}
