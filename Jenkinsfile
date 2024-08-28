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
                    sh 'scp -i "/home/jenkins/.ssh/id_rsa" -r /home/jenkins/jenkins-node ubuntu@13.232.118.241://home/ubuntu/'

                    // Change ownership of remote directory
                    sh 'ssh -i "/home/jenkins/.ssh/id_rsa" ubuntu@13.232.118.241 "sudo chown -R ubuntu:ubuntu /home/ubuntu/jenkins-node"'

                    // Restart PM2 process
                    sh 'ssh -i "/home/jenkins/.ssh/id_rsa" ubuntu@13.232.118.241 "sudo chmod -R 777 /home/ubuntu/jenkins-node/.git/ && pm2 stop /home/ubuntu/jenkins-node/index.js && pm2 start /home/ubuntu/jenkins-node/index.js"'
                }
            }
        }
    }
}

