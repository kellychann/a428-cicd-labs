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
                        id: 'userInput', 
                        message: 'Lanjutkan ke tahap Deploy?', 
                        parameters: [
                            [$class: 'ChoiceParameter', 
                             choiceType: 'RadioChoiceDefinition', 
                             name: 'Proceed?', 
                             choices: 'ok\nabort']
                        ]
                    )
                    if (userInput == 'abort') {
                        error('Eksekusi Pipeline Selesai')
                    }
                }
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
