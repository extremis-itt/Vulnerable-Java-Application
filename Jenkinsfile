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
  stage('Dynamic application security testing'){
            steps{
                sh '''
                    nikto -h http://localhost:1337 | tee nikto.txt
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
            archiveArtifacts 'nikto.txt'

             // Send email notifications
        emailext(
            subject: "Build ${currentBuild.result}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
            body: """
            <p>Build URL: <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
            <p>Git Secrets Report: <a href="${env.BUILD_URL}/artifact/secrets.txt">secrets.txt</a></p>
            <p>SAST Output: <a href="${env.BUILD_URL}/artifact/SAST_output.txt">SAST_output.txt</a></p>
            <p>ZAP Report: <a href="${env.BUILD_URL}/artifact/output_ZAP.html">output_ZAP.html</a></p>
            """,
            to: "bluedustbb@gmail.com", // Add the recipient email address here
            attachLog: true, // Attach build log
            compressLog: true // Compress build log
        )
           
        }
    }
}
