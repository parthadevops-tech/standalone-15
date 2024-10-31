pipeline {
    agent any
    
    tools {
        nodejs 'v18.19.0' // Ensure this matches the NodeJS installation name in Jenkins
    }
    
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'master', url: 'git@github.com:parthadevops-tech/standalone-15.git'
            }
        }
        
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        
        stage('Build Angular App') {
            steps {
                sh 'npm run build --prod'
            }
        }

        stage('Deploy to Nginx/Apache') {
            steps {
                sshagent(['jenkins']) {
                    sh '''
                    rsync -avz --delete dist/STANDALONE-15/browser user@80:/var/www/html/
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}
