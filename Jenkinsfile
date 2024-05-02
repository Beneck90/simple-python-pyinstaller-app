pipeline {
    agent any
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Build') {
            steps {
                withEnv(['PATH+EXTRA=/usr/bin/python3.11']) {
                    sh 'python -m py_compile sources/add2vals.py sources/calc.py'
                    stash(name: 'compiled-results', includes: 'sources/*.py*')
                }
            }
        }
        stage('Test') {
            steps {
                withEnv(['PATH+EXTRA=/usr/bin/python3.11']) {
                    sh 'py.test --junit-xml test-reports/results.xml sources/test_calc.py'
                }
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                withEnv(['PATH+EXTRA=/usr/bin/python3.11']) {
                    sh "pyinstaller --onefile sources/add2vals.py"
                }
            }
            post {
                success {
                    archiveArtifacts 'dist/add2vals'
                }
            }
        }
    }
}

