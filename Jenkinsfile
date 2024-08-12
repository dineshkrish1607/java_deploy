pipeline {
    agent any  // Runs the pipeline on any available agent

    environment {
        REMOTE_SSH_CREDENTIALS_ID = 'Slave1'  // Jenkins SSH credentials ID
        REMOTE_HOST = '13.201.85.186'  // Remote server address (IP or hostname)
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code from GitHub...'
                git url: 'https://github.com/dineshkrish1607/java_deploy.git', branch: 'main'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project with Maven...'
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                script {
                    docker.build('my-sample-app:latest')
                }
            }
        }

        stage('Deploy to Remote Server') {
            steps {
                echo 'Deploying Docker container to remote server...'
                sshagent([env.REMOTE_SSH_CREDENTIALS_ID]) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no user@${REMOTE_HOST} << 'EOF'
                        echo "Pulling Docker image on remote server..."
                        docker pull my-sample-app:latest
                        echo "Running Docker container on remote server..."
                        docker run -d -p 8080:8080 my-sample-app:latest
                    EOF
                    '''
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed'
        }
    }
}
