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
                    currentBuild.displayName = "Manual Approval"
                    input(message: 'Klik "Proceed" untuk melanjutkan ke tahap Deploy', ok: 'Proceed')
                    currentBuild.displayName = "Deploy"
                }
            }
        }
        stage('Deploy') {
            when {
                expression { currentBuild.displayName == 'Deploy' }
            }
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
        }
        stage('Pause') {
            steps {
                sh 'sleep 60' 
            }
        }
        stage('End Application') {
            steps {
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}
