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
                echo 'Deploying application to Staging Environment...'
                // Simulated deployment command
                sh '''
                echo "Deployment triggered successfully for staging!"
                '''
            }
        }
    }
    
    post {
        always {
            echo "Pipeline Execution Completed."
        }
        success {
            echo "Build Succeeded! (Simulated Email Notification)"
        }
        failure {
            echo "Build Failed! (Simulated Email Notification)"
        }
    }
}
