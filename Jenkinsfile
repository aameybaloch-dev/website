pipeline {
    agent any

    environment {
        IMAGE_NAME = "webapp"
        PROD_TAG = "webapp:prod"
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building Docker image'
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests'
                sh 'echo "Add your tests here"'
            }
        }

        stage('Deploy to Prod') {
            when {
                branch 'master'
            }
            steps {
                echo 'Deploying to Production'
                sh 'docker tag $IMAGE_NAME $PROD_TAG'
                sh 'docker run -d -p 80:80 $PROD_TAG'
            }
        }
    }
}
