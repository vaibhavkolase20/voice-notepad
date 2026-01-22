pipeline {
    agent { label 'agent-vinod' }

    stages {

        stage('Check User & Docker') {
            steps {
                sh '''
                echo "Current User:"
                whoami
                id

                echo "Docker Version (with sudo):"
                sudo docker --version
                '''
            }
        }

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/vaibhavkolase20/voice-notepad.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                sudo docker build -t voice-notepad-app .
                '''
            }
        }

        stage('Stop Old Container') {
            steps {
                sh '''
                sudo docker stop voice-notepad || true
                sudo docker rm voice-notepad || true
                '''
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                sudo docker run -d \
                  --name voice-notepad \
                  -p 3000:3000 \
                  voice-notepad-app
                '''
            }
        }

        stage('Verify Running Container') {
            steps {
                sh '''
                sudo docker ps
                '''
            }
        }
    }
}
