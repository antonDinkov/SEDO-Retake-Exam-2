pipeline {
    agent any

    options {
        timestamps()
    }

    environment {
        DOTNET_VERSION = '6.0.x'
    }

    stages {
        stage('Checkout Repo') {
            steps {
                checkout scm
            }
        }

        stage('Restore dependencies') {
            steps {
                bat 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                bat 'dotnet build --no-restore'
            }
        }

        stage('Test') {
            steps {
                bat 'dotnet test --verbosity normal'
            }
        }
    }

    post {
        failure {
            echo '❌ Build or tests failed!'
        }
        success {
            echo '✅ Build and tests completed successfully!'
        }
    }
}