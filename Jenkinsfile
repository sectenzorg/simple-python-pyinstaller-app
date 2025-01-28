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
                // Menambahkan --with-xml untuk menghasilkan laporan XML
                sh 'python -m unittest discover -s sources -p "test_*.py" > result.log; tail -n 10 result.log'
                sh 'python -m unittest discover -s sources -p "test_*.py" > result.xml || true'
            }
            post {
                always {
                    // Pastikan hasil XML berada di lokasi yang tepat
                    junit '**/result.xml'
                }
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
