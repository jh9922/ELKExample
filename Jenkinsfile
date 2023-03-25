node {
    def application = "springbootapp"

    stage('Clone repository') {
        checkout scm
    }

    stage('Build image') {
        app = docker.build("${application}:${BUILD_NUMBER}")
    }

    stage('Login to Docker Hub') {
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-username', 'dockerhub-password') {
            // Login to Docker Hub registry
        }
    }

    stage('Push image to Docker Hub') {
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-username', 'dockerhub-password') {
            // Push the Docker image to Docker Hub registry
            docker.image("${application}:${BUILD_NUMBER}").push()
        }
    }

    stage('Deploy') {
        sh ("docker run -d -p 81:8080 -v /var/log/:/var/log/ ${application}:${BUILD_NUMBER}")
    }
}
