node {
    stage('Checkout') {
        checkout scm
    }

    stage('Build') {
        steps {
            script {
                def dockerImage = docker.image('node:16-buster-slim').run('-p 3000:3000')

                try {
                    docker.image(dockerImage.id).inside {
                        sh 'npm install'
                    }
                } finally {
                    dockerImage.stop()
                }
            }
        }
    }

    stage('Test') {
        def dockerImage = docker.image('node:16-buster-slim').run('-p 3000:3000')

        docker.image(dockerImage.id).inside {
            sh './jenkins/scripts/test.sh'
        }
    }
}