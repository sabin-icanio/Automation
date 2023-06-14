pipeline {
    agent any

    stages {
        stage('Check for New Commit') {
            steps {
                script {
                    // Get the commit hashes
                    def currentCommit = sh(returnStdout: true, script: 'git rev-parse HEAD').trim()
                    def previousCommit = sh(returnStdout: true, script: 'git rev-parse HEAD^').trim()

                    // Compare the commit hashes
                    // if (currentCommit != previousCommit) {
                    //     echo "New commit detected, aborting pipeline..."
                    //     previousBuild.result = 'ABORTED'
                    //     error("Pipeline aborted due to new commit")
                    // }
                }
            }
        }

        stage('Build') {
            when {
                // Only execute this stage if no new commit is detected
                expression { previousBuild.result != 'ABORTED' }
            }
            steps {
                // Clone the Git repository
                git 'https://github.com/sabin-icanio/automation.git'

                // Install dependencies (if needed)
                sh 'pip install -r requirements.txt'

                // Build the project (replace with actual build command)
                sh 'python3 app.py'
            }
        }

        stage('Test') {
            when {
                // Only execute this stage if no new commit is detected
                expression { previousBuild.result != 'ABORTED' }
            }
            steps {
                // Run tests (replace with actual test command)
                sh 'python3 test.py'
            }
        }

        stage('Deploy') {
            when {
                // Only execute this stage if no new commit is detected
                expression { previousBuild.result != 'ABORTED' }
            }
            steps {
                // Deploy the project (replace with actual deployment command)
                sh 'python3 deploy.py'
            }
        }
    }

    post {
        success {
            // Clean up or perform any necessary post-build actions
            echo "Executing clean-up actions..."
            // Add your clean-up commands here
        }
        always {
            // Clean up or perform any necessary post-build actions
            echo "Always executing clean-up actions..."
            // Add any additional clean-up commands here
        }
    }
}
