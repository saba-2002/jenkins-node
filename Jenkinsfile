pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                // Clone the repository into the workspace
                git url: 'https://github.com/saba-2002/jenkins-node.git', branch: 'main'
            }
        }

        stage('Deploy to Remote Server') {
            steps {
                script {
                    // Print the current user
                    sh 'whoami'

                    // Define directories
                    def localDir = '/var/lib/jenkins/workspace/jenkins-node'
                    def deployDir = '/home/jenkins/jenkins-node'

                    // Remove existing directory if it exists
                    sh "rm -rf ${deployDir}"

                    // Copy files from workspace to /home/jenkins/node-pipelin
                    sh "cp -r ${localDir} /home/jenkins/"

                    // Deploy to remote server using rsync
                    sh 'rsync -avz -e "ssh -i /home/jenkins/.ssh/id_rsa" ${deployDir} ubuntu@3.110.142.123:/home/ubuntu/'

                    // Change ownership of remote directory
                    sh 'ssh -i "/home/jenkins/.ssh/id_rsa" ubuntu@3.110.142.123 "sudo chown -R ubuntu:ubuntu /home/ubuntu/jenkins-node"'

                    // Restart PM2 process
                    sh 'ssh -i "/home/jenkins/.ssh/id_rsa" ubuntu@3.110.142.123 "pm2 stop /home/ubuntu/jenkins-node/app.js && pm2 start /home/ubuntu/jenkins-node/app.js"'
                }
            }
        }
    }
}

