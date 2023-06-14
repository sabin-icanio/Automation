pipeline {

    agent any

        stages {

            stage("build"){
                steps {
                    sh 'ls'
                    sh 'pm2 delete app.py'
                    sh 'pm2 start "python3 app.py" --name python'
                }
            }
        }
}
