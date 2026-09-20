pipeline {
    agent any
    
    stages {
        stage('Checkout Code') {
            steps {
                cleanWs()
                git branch: 'master', url: 'https://github.com/rahoodsejal-wq/project-swiggy'
            }
        }
        
        stage('Install Dependencies') {
            steps {
                // Automatically installs node and npm if missing, then runs install
                sh '''
                    export DEBIAN_FRONTEND=noninteractive
                    if ! command -v npm &> /dev/null; then
                        echo "npm not found. Installing Node.js and npm..."
                        apt-get update && apt-get install -y nodejs npm
                    fi
                    npm install
                '''
            }
        }
        
        stage('Deploy to Node 1 via SCP') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'node1-ssh-key', keyFileVariable: 'SSH_KEY', usernameVariable: 'SSH_USER')]) {
                    sh '''
                        echo "Starting deployment to Node 1..."
                        scp -o StrictHostKeyChecking=no -i $SSH_KEY -r * ubuntu@3.110.221.180:/var/www/html/
                        echo "Deployment completed successfully!"
                    '''
                }
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline executed successfully and application is live!'
        }
        failure {
            echo 'Pipeline failed. Please check logs for details.'
        }
    }
}
