pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh "npm install"
                sh "npm run build"
            }
        }
        
        stage('Deploy to EC2 instance') {
            steps {
                script {
                    // Replace the following placeholders with your actual values
                    def sourceDir = "build/"
                    def remoteUser = "ubuntu"
                    def remoteHost = "65.2.10.148"
                    def remoteDir = "/usr/share/nginx/html/"

                    // Use SCP to copy the build folder to the remote EC2 instance
                    sh "scp -i ~/ubuntukey.pem -r ${sourceDir} ${remoteUser}@${remoteHost}:${remoteDir}"

                    // Optional: You can add additional commands to run on the remote EC2 instance after copying the files.
                    // For example, you might want to restart a web server or perform other setup tasks.
                    // Restart Nginx on the remote EC2 instance
                    sh "ssh -i  ~/ubuntukey.pem ${remoteUser}@${remoteHost} 'sudo systemctl restart nginx'"
                }
            }
        }
    }
}
