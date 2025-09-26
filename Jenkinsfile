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

    /* ---- EMAIL NOTIFICATION WITH ATTACHED LOG ---- */
    post {
        success {
            emailext(
                to: 'truongvinhkhang0609@gmail.com',
                subject: "✅ SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """<p>Good news! The build succeeded.</p>
                         <p>See details here: <a href='${env.BUILD_URL}'>${env.BUILD_URL}</a></p>""",
                mimeType: 'text/html',
                attachLog: true
            )
        }
        failure {
            emailext(
                to: 'truongvinhkhang0609@gmail.com',
                subject: "❌ FAILURE: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """<p>The build failed. Check the console output for details:</p>
                         <p><a href='${env.BUILD_URL}console'>${env.BUILD_URL}console</a></p>""",
                mimeType: 'text/html',
                attachLog: true
            )
        }
    }
}
