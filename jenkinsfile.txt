pipeline {
    agent any

    environment {
        GITHUB_TOKEN = credentials('github-token')  // GitHub Personal Access Token stored in Jenkins
        GITHUB_REPO = 'https://github.com/danielsimon254/gallery'  // GitHub repository URL
    }

    stages {
        stage('Push Jenkinsfile to GitHub') {
            steps {
                script {
                    // Push Jenkinsfile to GitHub (Heroku auto-deploys from GitHub)
                    sh 'git config --global user.email "simondaniel254@gmail.com"'
                    sh 'git config --global user.name "danielsimon254"'
                    sh 'git add Jenkinsfile'
                    sh 'git commit -m "Update Jenkinsfile for Heroku auto-deploy"'
                    sh 'git push https://${GITHUB_TOKEN}@github.com/danielsimon254/gallery.git master'
                }
            }
        }
    }
}
