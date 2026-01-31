pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "hshar/webapp:latest"
        APP_DIR = "/var/www/html"
    }
    stages {
        stage('Checkout') {
            steps {
                echo "Checking out the code"
                checkout([$class: 'GitSCM', 
                    branches: [[name: '*/${env.BRANCH_NAME}']],
                    userRemoteConfigs: [[url: 'https://github.com/aameybaloch-dev/website.git']]
                ])
            }
        }

        stage('Build') {
            steps {
                echo "Building Docker image"
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
                // Example test: check if website index exists
                sh 'if [ ! -f index.html ]; then echo "index.html missing!"; exit 1; fi'
            }
        }

        stage('Deploy to Production') {
            when {
                branch 'master'
            }
            steps {
                echo "Deploying to Production"
                // Stop any existing container and remove it
                sh '''
                    docker stop website || true
                    docker rm website || true
                    docker run -d --name website -p 80:80 -v $APP_DIR:/var/www/html $DOCKER_IMAGE
                '''
            }
        }
    }

    post {
        always {
            echo "Pipeline finished for branch ${env.BRANCH_NAME}"
        }
        success {
            echo "Pipeline succeeded!"
        }
        failure {
            echo "Pipeline failed!"
        }
    }
}
