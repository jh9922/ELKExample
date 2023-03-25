node {
	def application = "springbootapp"
	

	stage('Clone repository') {
		checkout scm
	}

	stage('Build image') {
		app = docker.build("${application}:${BUILD_NUMBER}")
	}

	stage('Push image') {
	  docker.withRegistry('https://hub.docker.com/', 'docker-hub') {
	    app.push("l3nnn/${application}:${BUILD_NUMBER}")
	  }
	}

	stage('Deploy') {
		sh ("docker run -d -p 81:8080 ${application}:${BUILD_NUMBER}")
	}
}
