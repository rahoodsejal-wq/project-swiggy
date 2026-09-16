pipeline {
    agent any
    stages {
        stage('Install Dependencies') {
            steps {
                sh "npm install"
            }
        }
        stage('Deploy to Node 1') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'node1-ssh-key', keyFileVariable: 'KEY', usernameVariable: 'USER')]) {
                    sh "scp -o StrictHostKeyChecking=no -i ${KEY} -r * ${USER}@3.110.221.180:/var/www/html/"
                }
            }
        }
    }
}
