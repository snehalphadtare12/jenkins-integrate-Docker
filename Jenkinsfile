pipeline {
agent any

```
environment {
    IMAGE_NAME = "apache-web"
    IMAGE_TAG  = "latest"
    CONTAINER_NAME = "apache-container"
}

stages {

    stage('Checkout Code') {
        steps {
            checkout scm
        }
    }

    stage('Build Docker Image') {
        steps {
            sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
        }
    }

    stage('Stop Old Container') {
        steps {
            sh '''
            if [ "$(docker ps -aq -f name=${CONTAINER_NAME})" ]; then
                docker stop ${CONTAINER_NAME}
                docker rm ${CONTAINER_NAME}
            fi
            '''
        }
    }

    stage('Run Docker Container') {
        steps {
            sh '''
            docker run -d \
            --name ${CONTAINER_NAME} \
            -p 8080:80 \
            ${IMAGE_NAME}:${IMAGE_TAG}
            '''
        }
    }
}

post {
    success {
        echo "Docker container deployed successfully"
    }
    failure {
        echo "Pipeline failed"
    }
    always {
        cleanWs()
    }
}
```

}
