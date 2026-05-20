pipeline {
    agent any

    environment {
        SERVER = "ubuntu@32.192.71.207"
    }

    stages {

        stage('Build') {
            steps {
                sh '''
                    mkdir -p build
                    cp index.html build/
                    cp style.css build/
                    cp script.js build/
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    scp -o StrictHostKeyChecking=no -r build/* $SERVER:/tmp/

                    ssh -o StrictHostKeyChecking=no $SERVER "
                    sudo rm -rf /var/www/html/*
                    sudo cp -r /tmp/* /var/www/html/
                    sudo systemctl restart apache2
                    "
                '''
            }
        }
    }

    post {
        success {
            echo 'Deployment successful'
        }

        failure {
            echo 'Deployment failed'
        }
    }
}
