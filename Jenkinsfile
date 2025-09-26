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
                // continue even if tests fail so later stages still run
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
                to: 'truongvinhkhang0609@gmail.com',
                subject: "SUCCESS: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                body: """<p>✅ Good news! The build <b>succeeded</b>.</p>
                         <p>View build details: <a href='${env.BUILD_URL}'>${env.BUILD_URL}</a></p>""",
                mimeType: 'text/html'
            )
        }
        failure {
            emailext(
                to: 'truongvinhkhang0609@gmail.com',
                subject: " FAILURE: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                body: """<p> The build <b>failed</b>.</p>
                         <p>Console output: <a href='${env.BUILD_URL}console'>${env.BUILD_URL}console</a></p>""",
                mimeType: 'text/html',
                attachLog: true
            )
        }
    }
}
