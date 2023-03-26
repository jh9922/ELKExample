node {

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')
        application = "springbootapp"
    }

    stage('Login to Docker Hub') {
        sh "echo $DOCKERHUB_CREDENTIALS | docker login -u ${DOCKERHUB_CREDENTIALS_USR} --password-stdin"
    }

    stage('Clone repository') {
        checkout scm
    }

    stage('Build image') {
        app = docker.build("${application}:${BUILD_NUMBER}")
    }

    stage('Push to Docker Hub') {
        steps {
            sh "docker tag ${application}:${BUILD_NUMBER} l3nnn/${application}:${BUILD_NUMBER}"
            sh "docker push l3nnn/${application}:${BUILD_NUMBER}"
        }
    }

    stage('Deploy to environment') {
        sh "docker run -d -p 81:8080 -v /var/log/:/var/log/ l3nnn/${application}:${BUILD_NUMBER}"
    }

    post {
        always {
            sh 'docker logout'
        }
    }
}
