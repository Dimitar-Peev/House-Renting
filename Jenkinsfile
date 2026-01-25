pipeline {
    agent any

    stages {
        stage('Checkout the repository') {
            steps {
                checkout scm
            }
        }

        stage('Restore the project') {
            steps {
                bat 'dotnet restore'
            }
        }


        stage('Build the project') {
            steps {
                bat 'dotnet build'
            }
        }

        stage('Run tests') {
            steps {
                bat 'dotnet test'
            }
        }
    }
    post {
        always {
            echo 'Pipeline completed'
        }
        success {
            echo 'Build succeeded'
        }
        failure {
            echo 'Build failed'
        }
    }
}