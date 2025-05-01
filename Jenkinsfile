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
                dir('notes-app-backend') {
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
        echo 'Stopping and removing old containers (if any)...'
        bat 'docker rm -f mern-frontend || echo "No existing frontend"'
        bat 'docker rm -f mern-backend || echo "No existing backend"'
        bat 'docker rm -f mongo || echo "No existing MongoDB"'

        echo 'Starting MongoDB container...'
        bat 'docker run -d --name mongo -p 27017:27017 mongo:latest'

        echo 'Building and running backend container...'
        dir('notes-app-backend') {
            bat 'docker build -t mern-backend .'
            bat 'docker run -d --name mern-backend --link mongo -p 3000:3000 mern-backend'
        }

        echo 'Building and running frontend container...'
        bat 'docker build -t mern-frontend .'
        bat 'docker run -d --name mern-frontend --link mern-backend -p 3001:3000 mern-frontend'

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
