node {
    properties([
        pipelineTriggers([pollSCM('*/2 * * * *')])
    ])

    stage('Build') {
        docker.image('node:16-buster-slim').inside('-p 3000:3000') {
            sh 'npm install'
        }
    }

    stage('Test') {
        docker.image('node:16-buster-slim').inside('-p 3000:3000') {
            sh './jenkins/scripts/test.sh'
        }
    }

    stage('Manual Approval') {
        input message: 'Lanjutkan ke tahap Deploy?'
    }

    stage('Deploy') {
        sh './jenkins/scripts/deliver.sh'
        sleep 60 // Tidur selama 60 detik
        sh './jenkins/scripts/kill.sh'
    }
}
