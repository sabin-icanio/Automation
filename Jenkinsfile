pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                script {
                    // Check if a previous build is running
                    def previousBuild = currentBuild.rawBuild.getPreviousBuild()
                    if (previousBuild != null) {
                        echo "Previous build found, killing it..."
                        previousBuild.executor.interrupt(Result.ABORTED)
                    }

                    // Clone the Git repository
                    git 'hhttps://github.com/sabin-icanio/Automation.git/'

                    // Build the Python project
                    sh 'python3 app.py'
                }
            }
        }
    }

    post {
        always {
            script {
                // Check if a new Git push occurred
                def currentCommit = sh(returnStdout: true, script: 'git rev-parse HEAD').trim()
                def previousCommit = sh(returnStdout: true, script: 'git rev-parse HEAD^').trim()

                if (currentCommit != previousCommit) {
                    echo "New Git push detected, killing current build..."
                    currentBuild.result = 'ABORTED'
                    error("Build aborted due to new Git push")
                }
            }
        }
    }
}

