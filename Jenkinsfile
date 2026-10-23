
pipeline {
    agent any

    stages {
        stage('Install Dependencies') {
            steps {
                sh 'npm ci'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t node-app-jenkins:latest .'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    docker rm -f node-app-jenkins || true
                    docker run -d \
                        --name node-app-jenkins \
                        --restart unless-stopped \
                        -p 3000:3000 \
                        node-app-jenkins:latest
                '''
            }
        }
    }

    post {
        success {
            echo 'CI/CD pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed. Check the logs.'
        }
    }
}
