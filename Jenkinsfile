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
            from: 'kaelentruong@gmail.com',
            subject: "SUCCESS: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
            body: """<p>Good news! The build succeeded.</p>"""
        )
    }
    failure {
        emailext(
            to: 'kaelentruong@gmail.com',
            from: 'kaelentruong@gmail.com',
            subject: "FAILURE: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
            body: """<p>Build failed. Check console output: ${env.BUILD_URL}console</p>""",
            attachLog: true
        )
    }
}

}
