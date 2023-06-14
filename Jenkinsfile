pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                script {
                    // Check if a previous build is running
                    def previousBuildNumber = sh(returnStdout: true, script: 'echo \$BUILD_NUMBER-1').trim()
                    def isPreviousBuildRunning = sh(returnStdout: true, script: "curl -s -X POST https://your-jenkins-instance/scriptText --data-urlencode 'script=Jenkins.instance.getBuild(\"${previousBuildNumber}\").isBuilding()'").trim()

                    if (isPreviousBuildRunning == 'true') {
                        echo "Previous build found, killing it..."
                        sh "curl -s -X POST https://your-jenkins-instance/scriptText --data-urlencode 'script=Jenkins.instance.getBuild(\"${previousBuildNumber}\").finish(hudson.model.Result.ABORTED)'"
                    }

                    // Clone the Git repository
                    git 'https://github.com/your/repo.git'

                    // Build the Python project
                    sh 'python build.py'
                }
            }
        }
    }

    post {
        always {
            script {
                try {
                    // Check if a new Git push occurred
                    def currentCommit = sh(returnStdout: true, script: 'git rev-parse HEAD').trim()
                    def previousCommit = sh(returnStdout: true, script: 'git rev-parse HEAD^').trim()

                    if (currentCommit != previousCommit) {
                        echo "New Git push detected, killing current build..."
                        currentBuild.result = 'ABORTED'
                        throw new AbortException("Build aborted due to new Git push")
                    }
                } catch (Exception e) {
                    echo "Handled exception: ${e.getMessage()}"
                    // Handle the exception gracefully, if desired
                    // For example, you can mark the build as unstable instead of failed
                    currentBuild.result = 'UNSTABLE'
                }
            }
        }
    }
}
