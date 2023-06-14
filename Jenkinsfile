pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout the Git repository
                git branch: 'main', url: 'https://github.com/sabin-icanio/Automation.git'
            }
        }

        stage('Build') {
            steps {
                // Execute your build commands here
                sh 'pip install -r requirement.txt'
                sh 'python3 app.py'
            }
        }
    }
}

