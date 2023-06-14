pipeline {
    agent any
    
    tools {nodejs "nodejs"}

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
                sh 'pip3 install -r requirements.txt'
                sh 'npm install pm2 -g'
                sh 'pm2 start . -i 3000"python3 app.py" --name python-web-api '           }
        }
    }
}

