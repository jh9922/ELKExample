node {
  def application = "springbootapp"

  stage('Clone repository') {
    checkout scm
  }

  stage('Build image') {
    app = docker.build("${application}:${BUILD_NUMBER}")
  }

  stage('Login to Dockerhub') {
    docker.withRegistry("https://index.docker.io/v1/", "docker-hub") {
      def login = docker.login(
        registry: "https://index.docker.io/v1/",
        username: "DOCKER_HUB_USERNAME",
        password: "DOCKER_HUB_PASSWORD"
      )
      if (login.status != "Login Succeeded") {
        error("Login to Dockerhub failed")
      }
    }
  }

  stage('Push image') {
    docker.withRegistry('https://index.docker.io/v1/', 'docker-hub') {
      app.push("DOCKER_HUB_USERNAME/${application}:${BUILD_NUMBER}")
    }
  }

  stage('Deploy') {
    sh ("docker run -d -p 81:8080 -v /var/log/:/var/log/ ${application}:${BUILD_NUMBER}")
  }
}
