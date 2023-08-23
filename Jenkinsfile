node {
    docker.image('node:16-buster-slim').inside('-p 3000:3000'){
        stage('Build') {
            checkout scm
            sh 'npm install'
        }
    }

    stage('Test') {
        try {
            dockerImage = docker.image('node:16-buster-slim').run('-p 3000:3000')

            docker.image(dockerImage.id).inside {
                sh './jenkins/scripts/test.sh'
            }
        } finally {
            dockerImage.stop()
        }
    }
}