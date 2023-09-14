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
                sh "trufflehog --json /opt/Vulnerable-Java-Application > trufflehog_output.json"
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
    }

    post {
        always {
            // Archive the Trufflehog results as a build artifact
            archiveArtifacts artifacts: 'trufflehog_output.json', onlyIfSuccessful: true
        }
    }
}
