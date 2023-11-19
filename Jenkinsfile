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
                input message: 'Lanjutkan ke tahap Deploy?', ok: 'Proceed', parameters: [boolean(defaultValue: false, description: 'Pilih Proceed untuk melanjutkan atau Cancel untuk menghentikan', name: 'Proceed')]
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