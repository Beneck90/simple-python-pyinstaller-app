pipeline {
    agent any
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Build') {
            steps {
                withEnv(['PATH+EXTRA=/usr/bin/python3.11']) {
                    sh 'python3 -m py_compile sources/add2vals.py sources/calc.py'
                    stash(name: 'compiled-results', includes: 'sources/*.py*')
                }
            }
        }
        stage('Test') {
            steps {
                withEnv(['PATH+EXTRA=/usr/bin/python3.11']) {
                    sh 'pytest --junit-xml test-reports/results.xml sources/test_calc.py'
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
        // Prepara el entorno con el PATH modificado para incluir la ruta del entorno virtual
        withEnv(["PATH=${env.WORKSPACE}/myenv/bin:${env.PATH}"]) {
            // Ejecuta pyinstaller desde el entorno virtual especificando la ruta completa
            sh "/myenv/bin/pyinstaller --onefile sources/add2vals.py"
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


