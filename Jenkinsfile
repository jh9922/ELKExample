node {
  def application = "springbootapp"

  stage('Clone repository') {
    checkout scm
  }

  stage('Build image') {
    app = sh(script: '/usr/bin/docker build -t ${application}:${BUILD_NUMBER} .', returnStdout: true).trim()
  }

  stage('Login to Dockerhub') {
    withCredentials([usernamePassword(credentialsId: 'leny', usernameVariable: 'l3nnn', passwordVariable: 'DOCKER_PASSWORD')]) {
      sh """
        /usr/bin/docker login -u l3nnn -p $DOCKER_PASSWORD
      """
    }
  }

  stage('Push image') {
    sh """
      /usr/bin/docker tag ${application}:${BUILD_NUMBER} l3nnn/${application}:${BUILD_NUMBER}
      /usr/bin/docker push l3nnn/${application}:${BUILD_NUMBER}
    """
  }

  stage('Deploy') {
    sh ("/usr/bin/docker run -d -p 81:8080 -v /var/log/:/var/log/ ${application}:${BUILD_NUMBER}")
  }
}
