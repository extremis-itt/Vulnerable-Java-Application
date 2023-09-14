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

     stage('Scan Code with Trufflehog') {
            steps {
                script {
                    // Run Trufflehog with the desired options
                    def trufflehogCommand = """trufflehog --regex /opt/Vulnerable-Java-Application > trufflehog"""
                    def trufflehogOutput = sh(script: trufflehogCommand, returnStatus: true)

                    // Check the exit status of Trufflehog
                    if (trufflehogOutput == 0) {
                        echo 'Trufflehog scan completed successfully'
                    } else {
                        error 'Trufflehog scan failed'
                    }
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

