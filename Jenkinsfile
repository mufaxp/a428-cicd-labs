node {
    stage('Build') {
        docker.image('node:16-buster-slim').inside('-p 3200:3200') {
            sh 'npm install'
        }
    }
    stage('Test') {
        docker.image('node:16-buster-slim').inside('-p 3200:3200') {
            sh './jenkins/scripts/test.sh'
        }
    }

    def deployApproval = env.DEPLOY_APPROVAL ?: 'not_approved'

    if (deployApproval == 'not_approved') {
        stage('Manual Approval') {
            def userApproval = input(
                id: 'deploy-approval',
                message: 'Lanjutkan ke tahap Deploy?',
                parameters: [
                    [$class: 'ButtonParameterDefinition', name: 'Proceed', value: 'Proceed'],
                    [$class: 'ButtonParameterDefinition', name: 'Abort', value: 'Abort']
                ],
                submitterParameter: 'ACTION'
            )

            if (userApproval == 'Proceed') {
                deployApproval = 'approved'
            } else {
                echo 'Persetujuan ditolak. Pipeline dihentikan...'
                currentBuild.result = 'ABORTED'
                return
            }
        }
    }
    stage('Deploy') {
        docker.image('node:16-buster-slim').inside('-p 3200:3200') {
            sh './jenkins/scripts/deliver.sh'
            sh 'sleep 60'
            sh './jenkins/scripts/kill.sh'
        }
    }

    env.DEPLOY_APPROVAL = deployApproval
}