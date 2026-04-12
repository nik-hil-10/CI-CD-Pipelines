pipeline {
    agent any

    environment {
        VENV_DIR = "${WORKSPACE}/venv"
    }

    stages {
        stage('Build') {
            steps {
                echo 'Creating Virtual Environment and Installing Dependencies...'
                sh '''
                python3 -m venv ${VENV_DIR}
                ${VENV_DIR}/bin/pip install --upgrade pip
                ${VENV_DIR}/bin/pip install -r requirements.txt
                '''
            }
        }
        stage('Test') {
            steps {
                echo 'Running Unit Tests using Pytest...'
                sh '''
                ${VENV_DIR}/bin/pytest test_app.py -v
                '''
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying application to AWS Staging Environment...'
                // Using credentials binding to fetch the key securely
                withCredentials([sshUserPrivateKey(credentialsId: 'aws-staging-key', keyFileVariable: 'SSH_KEY', usernameVariable: 'SSH_USER')]) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no -i $SSH_KEY $SSH_USER@13.204.75.214 "mkdir -p ~/flask-staging"
                    scp -o StrictHostKeyChecking=no -i $SSH_KEY app.py requirements.txt $SSH_USER@13.204.75.214:~/flask-staging/
                    '''
                }
            }
        }
    }
    
    post {
        always {
            echo "Pipeline Execution Completed."
        }
        success {
            echo "Build Succeeded!"
            mail to: 'admin@example.com',
                 subject: "SUCCESS: Jenkins Build Pipeline",
                 body: "The Jenkins CI/CD Pipeline completed successfully."
        }
        failure {
            echo "Build Failed!"
            mail to: 'admin@example.com',
                 subject: "FAILED: Jenkins Build Pipeline",
                 body: "The Jenkins CI/CD Pipeline encountered an error."
        }
    }
}
