node {
	def application = "springbootapp"
	def dockerhubaccountid = "fenrix"
	stage('Clone repository') {
		checkout scm
	}

	stage('Build image') {
		app = docker.build("${dockerhubaccountid}/${application}:${BUILD_NUMBER}")
	}

	stage('Push image') {
		withDockerRegistry([ credentialsId: "dockerHub", url: "" ]) {
		app.push()
		app.push("latest")
		}
	}

	stage('Remove running image') {
		sh ("docker rm ${application} -f ")
	}

	stage('Deploy') {
		sh ("docker run -d -p 81:8080 -v /var/log/:/var/log/ --name ${application} ${dockerhubaccountid}/${application}:${BUILD_NUMBER}")
	}
	
	stage('Remove old images') {
		// remove docker pld images
		sh("docker rmi `docker images ${dockerhubaccountid}/${application} -q` -f")
   }
}
