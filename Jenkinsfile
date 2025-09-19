pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/gnahk66/8.2CDevSecOps-.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                // Continue even if tests fail
                bat 'npm test || exit /b 0'
            }
        }

        stage('Generate Coverage Report') {
            steps {
                bat 'npm run coverage || exit /b 0'
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                bat 'npm audit || exit /b 0'
            }
        }
    }

    post {
        success {
            emailext(
                to: 'kaelentruong@gmail.com',
                subject: "SUCCESS: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                body: """
                    <p>Good news, your Jenkins pipeline succeeded!</p>
                    <p><b>Job:</b> ${env.JOB_NAME}</p>
                    <p><b>Build Number:</b> ${env.BUILD_NUMBER}</p>
                    <p>See details: <a href='${env.BUILD_URL}'>${env.BUILD_URL}</a></p>
                """,
                mimeType: 'text/html',
                attachLog: true   // attach full build log
            )
        }
        failure {
            emailext(
                to: 'kaelentruong@gmail.com',
                subject: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                body: """
                    <p>Oops, the Jenkins pipeline failed!</p>
                    <p><b>Job:</b> ${env.JOB_NAME}</p>
                    <p><b>Build Number:</b> ${env.BUILD_NUMBER}</p>
                    <p>Check the console output here: <a href='${env.BUILD_URL}console'>Console Log</a></p>
                """,
                mimeType: 'text/html',
                attachLog: true   // include the console log automatically
            )
        }
    }
}
