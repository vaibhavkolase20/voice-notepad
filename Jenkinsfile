pipeline {
    agent { label 'agent-vinod' }

    stages {

        stage('Fix Docker Permission') {
            steps {
                sh '''
                echo "Running as user:"
                whoami
                id

                echo "Adding jenkins user to docker group..."
                sudo usermod -aG docker jenkins || true

                echo "Restarting Docker service..."
                sudo systemctl restart docker

                echo "Restarting Jenkins service to apply group change..."
                sudo systemctl restart jenkins || true

                echo "Docker socket permission:"
                ls -l /var/run/docker.sock
                '''
            }
        }

        stage('Verify Docker Access') {
            steps {
                sh '''
                echo "Verifying Docker access..."
                docker --version
                docker ps
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
                docker build -t voice-notepad-app .
                '''
            }
        }

        stage('Stop Old Container') {
            steps {
                sh '''
                docker stop voice-notepad || true
                docker rm voice-notepad || true
                '''
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                docker run -d \
                  --name voice-notepad \
                  -p 3000:3000 \
                  voice-notepad-app
                '''
            }
        }
    }
}
