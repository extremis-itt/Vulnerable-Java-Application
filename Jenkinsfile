pipeline {
    agent any
    tools {
        maven 'Maven'
    }
    stages {
        stage('Initialize') {
            steps {
                script {
                    sh '''
                        echo "PATH = ${PATH}"
                        echo "M2_HOME = ${M2_HOME}"
                    '''
                }
            }
        }

        stage('Check-secrets-trufflehog'){
            steps {
                script{
                    sh 'rm trufflehog || true'
                    sh 'trufflehog --regex /opt/Vulnerable-Java-Application > trufflehog'
                    sh 'cat trufflehog'
                }
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
    }
}

