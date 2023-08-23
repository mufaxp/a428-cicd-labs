node {
    def dockerImage

    stage('Checkout') {
        checkout scm
    }

    stage('Build and Run') {
        dockerImage = docker.image('node:16-buster-slim').run('-p 3000:3000')
        
        try {
            // Inside the container
            docker.image(dockerImage.id).inside {
                sh 'npm install'
            }
        } finally {
            // Stop and clean up the container
            dockerImage.stop()
        }
    }
}
