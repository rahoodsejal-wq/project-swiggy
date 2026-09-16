pipeline {
    agent any
    stages {
        stage('clean workspace') {
            steps {
                cleanWs()
            }
        }
        stage('Install Dependencies') {
            steps {
                sh "npm install"
            }
        }
        stage('Deploy to Node 1') {
            steps {
                sshagent(['node1-ssh-key']) {
                    sh "scp -o StrictHostKeyChecking=no -r * ubuntu@3.110.221.180:/var/www/html/"
                }
            }
        }
    }
}
