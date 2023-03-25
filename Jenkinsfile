node {
  def application = "springbootapp"

  stage('Clone repository') {
    checkout scm
  }

  stage('Build image') {
    app = docker.build("${application}:${BUILD_NUMBER}")
  }

  stage('Login to Dockerhub') {
    withCredentials([usernamePassword(credentialsId: 'leny', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
      docker.withRegistry("https://index.docker.io/v1/", "docker-hub") {
        def login = docker.login(username: l3nnn, password: 'dckr_pat_FCRGe5-9SwTdJIuU5wx0KPPnF-Y')
        if (login.status != "Login Succeeded") {
          error("Login to Dockerhub failed")
        }
      }
    }
  }

  stage('Push image') {
    docker.withRegistry('https://index.docker.io/v1/', 'docker-hub') {
      app.push("l3nnn/${application}:${BUILD_NUMBER}")
    }
  }

  stage('Deploy') {
    sh ("docker run -d -p 81:8080 -v /var/log/:/var/log/ ${application}:${BUILD_NUMBER}")
  }
}
