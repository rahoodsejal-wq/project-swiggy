pipeline {
    agent any

    stages {
        stage('Checkout from SCM') {
            steps {
                // Automatically checks out code from the configured Git repository
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                // Installs project packages using Node.js
                sh 'npm install'
            }
        }

        stage('Build React App') {
            steps {
                // Overrides CI=true so ESLint warnings don't fail the build
                sh 'CI=false npm run build'
            }
        }

        stage('Deploy to Apache Web Root') {
            steps {
                // Clears the old web root files and moves the new build assets into place
                sh '''
                    sudo rm -rf /var/www/html/*
                    sudo cp -r build/* /var/www/html/
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully! Swiggy app is live.'
        }
        failure {
            echo 'Pipeline failed. Check logs for details.'
        }
    }
}
