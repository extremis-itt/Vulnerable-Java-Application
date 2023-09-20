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
                   snyk test /opt/Vulnerable-Java-Application | tee snyk_output.txt
                '''
            }
        }

        stage('Static application security testing'){
            steps{
                sh '''
                   cd /opt/Vulnerable-Java-Application
                   mvn sonar:sonar -Dsonar.projectKey=Devsecops -Dsonar.host.url=https://sonarqube.intimetec.americas -Dsonar.login=7aae4cf0a39bebe881d4c671532923085dc84a60
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
