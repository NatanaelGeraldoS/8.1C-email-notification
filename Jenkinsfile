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
                sh '''
                mkdir -p logs
                npm test | tee logs/test.log'''
            }
            post {
                always {
                    emailext (
                        subject: "#${currentBuild.BUILD_NUMBER} - Test - ${currentBuild.currentResult}",
                        body: """Build Number: #${currentBuild.BUILD_NUMBER}
                Workspace: ${env.WORKSPACE}
                Duration: ${currentBuild.durationString}
                
                The test stage finished with status: ${currentBuild.currentResult}
                """,
                        to: "geraldonatanael84@gmail.com",
                        from: 'geraldonatanael84@gmail.com',
                        attachmentsPattern: "logs/test.log"
                    )
                }
            }
        }

        stage('Security Scan') {
            steps {
                sh '''
                mkdir -p logs
                npm audit --json > logs/security.log || true
                '''
            }
            post {
                always {
                    emailext (
                        subject: "#${currentBuild.BUILD_NUMBER} - Security Scan - ${currentBuild.currentResult}",
                        body: """Build Number: #${currentBuild.BUILD_NUMBER}
                Workspace: ${env.WORKSPACE}
                Duration: ${currentBuild.durationString}

                The Security Scan stage finished with status: ${currentBuild.currentResult}
                """,
                        to: "geraldonatanael84@gmail.com",
                        from: "geraldonatanael84@gmail.com",
                        attachmentsPattern: "logs/security.log"
                    )
                }

            }
        }
    }
}
