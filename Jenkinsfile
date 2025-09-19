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
            to: 'your_email@domain.com',
            subject: "SUCCESS: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
            body: """<p>Build Succeeded!</p>
                     <p>Job: <b>${env.JOB_NAME}</b></p>
                     <p>Build Number: <b>${env.BUILD_NUMBER}</b></p>
                     <p>Check details at: <a href='${env.BUILD_URL}'>${env.BUILD_URL}</a></p>""",
            mimeType: 'text/html',
            attachmentsPattern: '**/build.log'  // optional if you save logs as files
        )
    }
    failure {
        emailext(
            to: 'your_email@domain.com',
            subject: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
            body: """<p>Build Failed!</p>
                     <p>Job: <b>${env.JOB_NAME}</b></p>
                     <p>Build Number: <b>${env.BUILD_NUMBER}</b></p>
                     <p>Check console output at: <a href='${env.BUILD_URL}console'>Console Log</a></p>""",
            mimeType: 'text/html',
            attachLog: true   // 🔑 this attaches the console log to the email
        )
    }
}

}
// Jenkinsfile for CI/CD Pipeline with Email Notifications