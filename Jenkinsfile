node {
    properties([
        pipelineTriggers([pollSCM('*/2 * * * *')])
    ])

    docker.image('node:16-buster-slim').inside('-p 3000:3000') {
        stage('Build') {
            sh 'npm install'
        }

        stage('Test') {
            sh './jenkins/scripts/test.sh'
        }

        stage('Manual Approval') {
            input message: 'Lanjutkan ke tahap Deploy?'
        }

        stage('Deploy') {
            sh './jenkins/scripts/deliver.sh'
            sleep 60
            sh './jenkins/scripts/kill.sh'
        }
    }
}