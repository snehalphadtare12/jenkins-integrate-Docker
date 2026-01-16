pipeline {
    agent any

    environment {
        IMAGE_NAME = "jenkins-html-app"
        CONTAINER_NAME = "html_container"
    }

    stages {

        stage('Check & Install Docker') {
            steps {
                sh '''
                if ! command -v docker >/dev/null 2>&1; then
                    echo "Docker not found. Installing..."
                    sudo apt update
                    sudo apt install -y docker.io
                    sudo systemctl start docker
                    sudo systemctl enable docker
                    sudo usermod -aG docker jenkins
                else
                    echo "Docker already installed"
                fi
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Stop Old Container') {
            steps {
                sh '''
                if [ "$(docker ps -aq -f name=$CONTAINER_NAME)" ]; then
                    docker stop $CONTAINER_NAME
                    docker rm $CONTAINER_NAME
                fi
                '''
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                docker run -d \
                --name $CONTAINER_NAME \
                -p 8081:80 \
                $IMAGE_NAME
                '''
            }
        }
    }

    post {
        success {
            echo "Application deployed successfully!"
        }
    }
}
