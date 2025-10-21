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

        stage('Setup .NET') {
            steps {
                bat '''
                echo Installing .NET SDK %DOTNET_VERSION% ...
                curl -L -o dotnet-install.ps1 https://dot.net/v1/dotnet-install.ps1
                powershell -ExecutionPolicy Bypass -File dotnet-install.ps1 -Channel 6.0
                setx PATH "%PATH%;%USERPROFILE%\\.dotnet"
                dotnet --version
                '''
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