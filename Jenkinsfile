pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/GordeYash/web-ci-demo.git'
            }
        }

        stage('Build Application') {
            steps {
                sh '''
                    echo "Building web application..."
                    mkdir -p build
                    cp -r index.html Jenkinsfile build/
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
                    sudo mkdir -p /var/www/html/web-app
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
        echo "Build & Deployment Successful!"
    }
    failure {
        echo "Build Failed!"
    }
}

}
