pipeline {
    agent any
    stages {
        stage('Test Python Access') {
            steps {
                withEnv(['PATH+EXTRA=/usr/bin/python3']) {
                    sh 'python3 -c "print(\'Hola, mundo!\')"'
                }
            }
        }
    }
}


