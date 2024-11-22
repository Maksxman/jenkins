pipeline {
    agent any
    environment {
        APACHE_LOG = '/var/log/apache2/access.log'
    }
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Maksxman/jenkins.git'
            }
        }
        stage('Install Apache Server') {
            steps {
                sh '''
                sudo apt update
                sudo apt install -y apache2
                sudo systemctl start apache2
                sudo systemctl enable apache2
                '''
            }
        }
        stage('Check Logs for Errors') {
            steps {
                script {
                    def errors = sh(script: "sudo grep -E 'HTTP/[0-9]+\\.\\d+\" [45][0-9]{2}' $APACHE_LOG", returnStatus: true)
                    if (errors == 0) {
                        echo "Found 4xx/5xx errors in logs!"
                    } else {
                        echo "No 4xx/5xx errors found in logs."
                    }
                }
            }
        }
    }
    post {
        always {
            echo "Pipeline execution complete."
        }
    }
}
