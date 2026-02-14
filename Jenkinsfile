pipeline {
    agent any

    stages {
        stage('Checkout the repository') {
            steps {
                checkout scm
            }

            post {
                always {
                    echo """
                    ================ CHECKOUT STAGE FINISHED ================
                    Job:        ${env.JOB_NAME}
                    Build:      #${env.BUILD_NUMBER}
                    Branch:     ${env.BRANCH_NAME}
                    URL:        ${env.BUILD_URL}

                    Git output is available in the log above.
                    =========================================================
                    """
                }

                success {
                    echo """
                    ✔ CHECKOUT SUCCESS

                    Source code was successfully retrieved from the repository.
                    Pipeline will continue with dependency restore.
                    """
                }

                failure {
                    echo """
                    ✖ CHECKOUT FAILED

                    Jenkins could not download the source code.

                    Possible reasons:
                     - wrong Git credentials
                     - missing repository permissions
                     - incorrect branch name
                     - repository unavailable

                    What to check:
                      • 'authentication failed'
                      • 'repository not found'
                      • 'permission denied'

                    Build details:
                    ${env.BUILD_URL}
                    """
                }
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