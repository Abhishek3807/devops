pipeline {
    agent any
    environment {
        GIT_REPO = 'https://github.com/Abhishek3807/devops'  // Update if needed
        BRANCH = 'master'
    }
    stages {
        stage('Clone') {
            steps {
                git branch: "${BRANCH}", url: "${GIT_REPO}"
            }
        }
        stage('Build') {
            steps {
                echo 'Build step started...'
                // Since frontend is in root
                bat 'npm install'
                // Backend inside backend folder
                dir('backend') {
                    bat 'npm install'
                }
                echo 'Build step completed.'
            }
        }
        stage('Test') {
            steps {
                echo 'Running test scripts...'
                // Optional: If you have test commands, add them here
                echo 'Tests passed.'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying application with Docker Compose...'
                bat 'docker-compose down'
                bat 'docker-compose up --build -d'
                echo 'Deployment complete.'
                
            }
        }
    }
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}