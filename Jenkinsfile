pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/<username>/<repo>.git'
            }
        }

        stage('Build Application') {
            steps {
                sh '''
                    echo "Building web application..."
                    mkdir -p build
                    cp -r * build/
                '''
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'build/**', fingerprint: true
            }
        }
              stage('Deploy to Local Web Server') {
            steps {
                sh '''
                    echo "Deploying to local server..."
                    sudo rm -rf /var/www/html/web-app/*
                    sudo cp -r build/* /var/www/html/web-app/
                    sudo systemctl restart apache2 || sudo systemctl restart nginx
                    echo "Deployment completed at $(date)"
                '''
            }
        }

    }

    post {
        success {
            githubNotify context: 'CI/CD Pipeline', description: 'Build succeeded!', status: 'SUCCESS'
        }
        failure {
            githubNotify context: 'CI/CD Pipeline', description: 'Build failed!', status: 'FAILURE'
        }
    }
}
