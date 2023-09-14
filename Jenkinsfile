pipeline {
    agent any

    tools {
        maven 'Maven'
    }

    stages {
        stage('Initialize') {
            steps {
                script {
                    sh """
                        echo "PATH = ${PATH}"
                        echo "M2_HOME = ${M2_HOME}"
                    """
                }
            }
        }

        stage('Scan Code with Trufflehog') {
            steps {
                sh "trufflehog --json /opt/Vulnerable-Java-Application"
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
    }
