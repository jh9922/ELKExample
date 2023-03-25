node {
  env.PATH = "/usr/bin:${env.PATH}"
  def application = "springbootapp"

  stage('Clone repository') {
    checkout scm
  }

  stage('Build image') {
    app = docker.build("${application}:${BUILD_NUMBER}")
  }

  stage('Login to Dockerhub') {
    withCredentials([usernamePassword(credentialsId: 'leny', usernameVariable: 'l3nnn', passwordVariable: 'dckr_pat_FCRGe5-9SwTdJIuU5wx0KPPnF-Y')]) {
      docker.withRegistry("https://index.docker.io/v1/", "docker-hub") {
        def login = docker.login(username: DOCKER_USERNAME, password: DOCKER_PASSWORD)
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
