pipeline {

    agent any

        stages {

            stage("build"){
                steps {
                    sh 'ls'
                    echo "hi"
                    sh 'pm2 delete app.py'
                    sh 'pm2 start "python3 app.py" --name python'
                }
            }
        }
}
