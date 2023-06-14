pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                script {
                    // Check if a previous build is running
                    def previousBuildNumber = sh(returnStdout: true, script: 'echo \$BUILD_NUMBER-1').trim()
                    def isPreviousBuildRunning = sh(returnStatus: true, script: "jenkins model-build-is-running $previousBuildNumber") == 0

                    if (isPreviousBuildRunning) {
                        echo "Previous build found, killing it..."
                        sh "jenkins model-build-abort $previousBuildNumber"
                    }

                    // Clone the Git repository
                    git 'https://github.com/sabin-icanio/Automation.git/'

                    // Build the Python project
                    sh 'python build.py'
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
