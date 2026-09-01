pipeline {
    agent any

    options {
        timestamps()
        skipDefaultCheckout(true)
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Lint') {
            steps {
                sh 'ruff check app tests'
                sh 'ruff format --check app tests'
            }
        }

        stage('Unit tests') {
            steps {
                sh 'python3 -m pytest --junitxml=test-results.xml'
            }
        }

        stage('Security scan') {
            steps {
                sh 'bandit -r app'
            }
        }
    }

    post {
        always {
            junit allowEmptyResults: true, testResults: 'test-results.xml'
        }
    }
}
