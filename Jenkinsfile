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
                input message: 'Lanjutkan ke tahap Deploy?', ok: 'Proceed', parameters: [
                    choice(choices: ['Proceed', 'Abort'], description: 'Pilih untuk melanjutkan atau menghentikan eksekusi pipeline')
                ]
            }
        }
        stage('Deploy') { 
            steps {
                sh './jenkins/scripts/deliver.sh' 
                sh 'echo "Jeda 1 menit"'
                sh "sleep 60"
                sh './jenkins/scripts/kill.sh' 
            }
        }
    }
}
