
pipeline {
    agent any

    options {
        timestamps()
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out source code'
                checkout scm
            }
        }

        stage('Environment Setup') {
            steps {
                sh '''
                    echo "Checking Python version"
                    python3 --version

                    echo "Creating virtual environment"
                    python3 -m venv .venv

                    echo "Installing dependencies"
                    .venv/bin/pip install -r requirements.txt
                '''
            }
        }

        stage('Unit Testing') {
            steps {
                sh '''
                    mkdir -p reports

                    .venv/bin/pytest -v \
                    --junitxml=reports/pytest-results.xml
                '''
            }
        }
    }

    post {
        always {
            junit allowEmptyResults: true,
                  testResults: 'reports/pytest-results.xml'
        }

        success {
            echo 'CI Pipeline completed successfully!'
        }

        failure {
            echo 'CI Pipeline failed. Check the console output.'
        }
    }
}
