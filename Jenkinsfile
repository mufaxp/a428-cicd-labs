node {
    stage('Checkout') {
        checkout scm
    }

    stage('Build') {
        def dockerImage = docker.image('node:16-buster-slim').run('-p 3000:3000')

        docker.image(dockerImage.id).inside {
            sh 'npm install'
        }
    }

    stage('Test') {
        def dockerImage = docker.image('node:16-buster-slim').run('-p 3000:3000')

        docker.image(dockerImage.id).inside {
            sh './jenkins/scripts/test.sh'
        }
    }
}