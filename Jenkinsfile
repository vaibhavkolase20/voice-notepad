pipeline {
    agent { label 'agent-vinod' }

    stages {

        stage('Check Docker') {
            steps {
                sh '''
                if command -v docker >/dev/null 2>&1; then
                  echo "Docker already installed"
                  docker --version
                else
                  echo "Docker not installed"
                fi
                '''
            }
        }

        stage('Install Docker') {
            steps {
                sh '''
                if ! command -v docker >/dev/null 2>&1; then
                  echo "Installing Docker..."

                  sudo apt update -y
                  sudo apt install -y docker.io

                  sudo systemctl start docker
                  sudo systemctl enable docker

                  sudo usermod -aG docker ubuntu

                  echo "Docker installation completed"
                else
                  echo "Docker already present, skipping install"
                fi
                '''
            }
        }

        stage('Verify Docker') {
            steps {
                sh '''
                docker --version || sudo docker --version
                sudo docker ps
                '''
            }
        }

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                url: 'https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t digital-notepad-app .
                '''
            }
        }

        stage('Stop Old Container') {
            steps {
                sh '''
                docker stop digital-notepad || true
                docker rm digital-notepad || true
                '''
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                docker run -d \
                --name digital-notepad \
                -p 3000:3000 \
                digital-notepad-app
                '''
            }
        }
    }
}
