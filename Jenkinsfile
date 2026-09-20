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
        sh '''
            echo "Setting up Node.js locally..."
            curl -O https://nodejs.org/dist/v18.16.0/node-v18.16.0-linux-x64.tar.xz
            tar -xf node-v18.16.0-linux-x64.tar.xz
            export PATH=$PWD/node-v18.16.0-linux-x64/bin:$PATH
            node -v
            npm -v
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
