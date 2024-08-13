pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/jaydeep123s/mycompany.git'
            }
        }
        
        stage('Build') {
            steps {
                sh './gradlew build'
            }
        }
        
        stage('Test') {
            steps {
                sh './gradlew test'
            }
        }
        
        stage('Deploy') {
            steps {
                sshagent(['your-ssh-credentials-id']) {
                    sh '''
                    scp build/libs/your-app.jar ec2-user@your-web-server-ip:/path/to/deploy
                    ssh ec2-user@your-web-server-ip "sudo systemctl restart your-app-service"
                    '''
                }
            }
        }
    }

    post {
        always {
            junit 'build/test-results/test/*.xml'
        }
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Build or tests failed.'
        }
    }
}
