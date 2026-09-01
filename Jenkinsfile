pipeline {
    agent any

    options {
        timestamps()
        skipDefaultCheckout(true)
    }
    
    triggers {
    pollSCM('H/5 * * * *')
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
        
        stage('Terraform validation') {
            steps {
                sh 'terraform fmt -check'
                sh 'terraform init -backend=false -input=false'
                sh 'terraform validate -no-color'
            }
        }
    }
}
