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
                sh "trufflehog --json /opt/Vulnerable-Java-Application | tee trufflehog-output.txt"
            }
        }

        stage('scan code with snyk for dependencies'){
            steps{
                sh '''
                   cd /opt/Vulnerable-Java-Application
                   snyk test | tee snyk_output.txt
                '''
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
            archiveArtifacts 'trufflehog-output.txt'
            archiveArtifacts 'snyk_output.txt'
           
        }
    }
}
