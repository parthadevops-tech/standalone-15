pipeline {
    agent any
    
    tools {
        NodeJS 'NodeJS' // Ensure this matches the NodeJS installation name in Jenkins
    }
    
    environment {
        BUILD_DIR = "/var/lib/jenkins/workspace/angularapp-jenkins/dist/standalone-component15/browser"
        APACHE_DEPLOY_DIR = "/var/www/html/standalone-app"
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

        stage('Deploy to Apache') {
            steps {
                script {
                    // Check if the build directory exists
                    if (fileExists(env.BUILD_DIR)) {
                        echo "Build directory found, deploying to Apache server..."
                        
                        // Remove the existing files in the Apache deployment directory
                        sh "sudo rm -rf ${env.APACHE_DEPLOY_DIR}/*"
                        
                        // Copy the newly built files to the Apache deployment directory
                        sh "sudo cp -r ${env.BUILD_DIR}/* ${env.APACHE_DEPLOY_DIR}/"
                        
                        // Set the correct permissions for the files
                        sh "sudo chown -R www-data:www-data ${env.APACHE_DEPLOY_DIR}"
                        sh "sudo chmod -R 755 ${env.APACHE_DEPLOY_DIR}"
                        
                        // Restart Apache server
                        sh 'sudo systemctl restart apache2'
                        
                        echo "Deployment complete!"
                    } else {
                        error("Build directory not found! Deployment aborted.")
                    }
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
