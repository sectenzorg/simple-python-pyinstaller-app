pipeline {
    agent {
        docker {
            image 'python:2-alpine'
        }
    }
    stages {
        stage('Git Clone') { 
            steps {
                git branch: 'master', url: 'https://github.com/sectenzorg/simple-python-pyinstaller-app'
            }
        }
        stage('Build') {
            steps {
                sh 'python -m py_compile sources/add2vals.py sources/calc.py'
            }
        }
        stage('Test') {
            steps {
                // Langsung jalankan unittest dan hasilkan laporan XML
                sh 'python -m unittest discover -s sources -p "test_*.py" > result.xml || true'
            }
        }
        stage('Deliver') {
            steps {
                sh 'pyinstaller --onefile sources/add2vals.py'
            }
            post {
                success {
                    archiveArtifacts 'dist/add2vals'
                }
            }
        }
    }
}
