pipeline {
    agent any
    tools { nodejs 'NodeJS' }

    stages {
        stage('Build') {
            steps {
                sh 'npm install'
                sh 'npm run build'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
            post {
                always {
                    emailext (
                        subject: "Test - ${currentBuild.currentResult}",
                        body: "The test stage finished with status: ${currentBuild.currentResult}",
                        to: "s225000297@deakin.edu.au",
                        attachmentsPattern: "logs/test.log"
                    )
                }
            }
        }

        stage('Security Scan') {
            steps {
                sh 'npm audit --json > logs/security.log'
            }
            post {
                always {
                    emailext (
                        subject: "Security Scan - ${currentBuild.currentResult}",
                        body: "The Security Scan stage finished with status: ${currentBuild.currentResult}",
                        to: "s225000297@deakin.edu.au",
                        attachmentsPattern: "logs/security.log"
                    )
                }
            }
        }
    }
}
