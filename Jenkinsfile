pipeline {
    agent any

    environment {
        SERVER = "ubuntu@32.192.71.207"
    }

    stages {

        stage('Prepare Website') {
            steps {
                sh '''
                    mkdir -p build
                    cp index.html build/
                    cp style.css build/ || true
                    cp script.js build/ || true
                '''
            }
        }

        stage('Deploy to EC2') {
            steps {
                sh '''
                    ssh -o StrictHostKeyChecking=no $SERVER '
                        sudo mkdir -p /var/www/html
                    '

                    scp -o StrictHostKeyChecking=no -r build/* $SERVER:/tmp/

                    ssh -o StrictHostKeyChecking=no $SERVER '
                        sudo rm -rf /var/www/html/*
                        sudo cp -r /tmp/* /var/www/html/
                        sudo systemctl restart apache2
                    '
                '''
            }
        }
    }

    post {
        success {
            echo 'Website deployed successfully'
        }

        failure {
            echo 'Deployment failed'
        }
    }
}
