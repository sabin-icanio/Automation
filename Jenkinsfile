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
                sh 'PORT=5000 pm2 start -i "python3 app.py" --name python-web-api ' 
                sh 'pm2 save'
            }
        }
    }
}

