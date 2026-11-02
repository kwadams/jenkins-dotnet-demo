
pipeline {
    agent { label 'dotnet10' }

    options {
        timestamps()
        skipDefaultCheckout()
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Verify SDK') {
            steps {
                sh 'dotnet --version'
            }
        }

        stage('Restore') {
            steps {
                sh 'dotnet restore DemoApp/DemoApp.csproj'
            }
        }

        stage('Build') {
            steps {
                sh 'dotnet build DemoApp/DemoApp.csproj -c Release --no-restore'
            }
        }

        stage('Run') {
            steps {
                sh 'dotnet run --project DemoApp/DemoApp.csproj -c Release --no-build'
            }
        }

        stage('Publish') {
            steps {
                sh 'dotnet publish DemoApp/DemoApp.csproj -c Release --no-restore -o publish'
            }
        }
    }

    post {
        success {
            archiveArtifacts artifacts: 'publish/**',
                             fingerprint: true
            echo 'Pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed. Check Console Output.'
        }
    }
}
