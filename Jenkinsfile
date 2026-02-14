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

            post {
                success {
                    echo "✔ Dependencies restored successfully (.NET packages downloaded)."
                }
                
                failure {
                    echo """
✖ RESTORE FAILED

NuGet packages could not be restored.

Common causes:
 - no internet access from Jenkins agent
 - NuGet.org outage
 - corrupted cache
 - missing SDK version

Try:
  dotnet nuget locals all --clear
"""
                }
            }
        }

        stage('Build the project') {
            steps {
                bat 'dotnet build'
            }
            
            post {
                success {
                    echo "✔ BUILD SUCCESS — Project compiled without errors."
                }

                failure {
                    echo """
✖ BUILD FAILED

Compilation errors detected.

What to do:
 - scroll up in the log
 - look for the FIRST compiler error (the rest are consequences)
 - usually marked as: error CSxxxx

Tip:
Do NOT debug the last error. Always fix the first one.
"""
                }
            }
        }

        stage('Run tests') {
            steps {
                bat 'dotnet test'
            }

            post {
                success {
                    echo """
✔ TESTS PASSED

All unit tests completed successfully.
Application behavior is validated.
"""
                }

                unstable {
                    echo """
⚠ TESTS FAILED

The build compiled successfully, but some tests did not pass.

This usually means:
 - logic bug
 - changed behavior
 - flaky test

Check the test results and console output:
${env.BUILD_URL}
"""
                }

                failure {
                    echo """
✖ TEST EXECUTION FAILED

The test runner crashed or could not start.

Possible causes:
 - missing runtime
 - incompatible SDK
 - test project misconfiguration
"""
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed'
        }
        success {
            echo 'Build succeeded'
			cleanWs()
        }
        failure {
            echo 'Build failed, keeping workspace for debugging'
        }
    }
}