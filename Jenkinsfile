pipeline {
    agent {
        docker {
            image 'node:16-buster-slim' 
            args '-p 3000:3000' 
        }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'npm install'
            }
        }
        stage('Test') { 
            steps {
                sh './jenkins/scripts/test.sh' 
            }
        }
        stage('Manual Approval') {
            steps {
                script {
                    def userInput = input(
                        message: 'Lanjutkan deploy? (Klik "Proceed" untuk melanjutkan)',
                        ok: 'Proceed'
                    )
                    if (userInput != 'Proceed') {
                        error('Pengguna memilih untuk tidak melanjutkan. Menghentikan eksekusi pipeline.')
                    }
                }
            }
        }     
        stage('Deploy') {
            steps {
                sh './jenkins/scripts/deliver.sh'
                script {
                    sleep(time: 1, unit: 'MINUTES')
                }
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}