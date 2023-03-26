node {
    def application = "springbootapp"

    stage('Clone repository') {
        checkout scm
    }

    stage('Build image') {
        app = docker.build("${application}:${BUILD_NUMBER}")
    }

    stage('Deploy') {
        sh("docker run -d -p 81:8080 -v /var/log/:/var/log/ ${application}:${BUILD_NUMBER}")
    }

    stage('Tag and Push') {
        sh("docker tag ${application}:${BUILD_NUMBER} l3nnn/${application}")
        sh("docker push l3nnn/${application}")
    }
}
