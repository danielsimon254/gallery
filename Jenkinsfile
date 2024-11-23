pipeline {
    agent any

    environment {
        HEROKU_API_KEY = credentials('heroku') // Heroku API key ID from Jenkins credentials
        HEROKU_APP_NAME = 'danielsimon-app'           // Your Heroku app name
        GIT_REPO = 'https://github.com/danielsimon254/gallery.git' // Your GitHub repo
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: "${GIT_REPO}" // Replace 'main' if using a different branch
            }
        }

        stage('Install Heroku CLI') {
            steps {
                sh '''
                if ! command -v heroku &> /dev/null; then
                    curl https://cli-assets.heroku.com/install.sh | sh
                fi
                '''
            }
        }

        stage('Login to Heroku') {
            steps {
                sh 'echo $HEROKU_API_KEY | heroku auth:token | heroku auth:whoami'
            }
        }

        stage('Deploy to Heroku') {
            steps {
                sh '''
                git remote add heroku https://git.heroku.com/${HEROKU_APP_NAME}.git || true
                git push heroku main --force
                '''
            }
        }
    }

    post {
        success {
            echo 'Deployment to Heroku was successful!'
        }
        failure {
            echo 'Deployment to Heroku failed. Check the logs for details.'
        }
    }
}