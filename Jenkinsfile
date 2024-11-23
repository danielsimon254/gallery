pipeline {
    agent any

    environment {
        GITHUB_TOKEN = credentials('github-token')  // GitHub Personal Access Token stored in Jenkins
        HEROKU_API_KEY = credentials('HRKU-f5bd0574-c37f-4e52-9fbf-c9cc10470167')  // Heroku API Key
        HEROKU_APP_NAME = 'danielsimon-app'  // Heroku app name
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm  // Checkout the repository from GitHub
            }
        }

        stage('Push Jenkinsfile to GitHub') {
            steps {
                script {
                    // Set up Git configuration for committing to GitHub
                    sh 'git config --global user.email "simondaniel254@gmail.com"'
                    sh 'git config --global user.name "danielsimon254"'

                    // Ensure we are on the correct branch
                    sh 'git checkout master'  // You can change this to 'main' or another branch if needed

                    // Check if there are any changes to commit
                    def changes = sh(script: 'git status --porcelain', returnStdout: true).trim()
                    if (changes) {
                        // If changes exist, add and commit them
                        sh 'git add Jenkinsfile'
                        sh 'git commit -m "Update Jenkinsfile for Heroku auto-deploy"'
                        // Push changes to GitHub using the GitHub token
                        sh 'git push https://${GITHUB_TOKEN}@github.com/danielsimon254/gallery.git master'  // Change 'master' if using a different branch
                    } else {
                        echo "No changes detected, skipping commit."
                    }
                }
            }
        }

        stage('Deploy to Heroku') {
            steps {
                script {
                    // Authenticate with Heroku using the API key
                    echo "Authenticating with Heroku..."

                    // Explicit Heroku authentication with API key
                    sh '''
                    echo $HEROKU_API_KEY | heroku auth:token
                    '''
                    
                    // Configure Heroku remote
                    echo "Configuring Heroku remote..."
                    sh '''
                    git remote remove heroku || true
                    git remote add heroku https://git.heroku.com/$HEROKU_APP_NAME.git
                    '''

                    // Push to Heroku master branch
                    echo "Pushing code to Heroku..."
                    sh 'git push heroku master'  // Push to Heroku's master branch
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
