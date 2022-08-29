node {
	stage("Checkout - AuthService"){
		git url: 'http://github.com/dipendrashrestha1281/teamCDauthServiceRepo'
	}
	
	stage("Gradle Build - AuthService"){
		sh 'gradle -b teamCDmccauthservice/build.gradle clean build'
	}
	
	stage ("Gradle Bootjar-Package - AuthService") {
        sh 'gradle -b teamCDmccauthservice/build.gradle bootjar'
    }
    
    stage ("Containerize the app-docker build - AuthService") {
        sh 'docker build --rm -t authapi:v1.0 .'
    }
    
    stage ("Inspect the docker image - AuthService"){
        sh "docker images authapi:v1.0"
        sh "docker inspect authapi:v1.0"
    }
    
    stage ("Run Docker container instance - AuthService"){
        sh "docker run -d --rm --name authapi -p 8081:8081 authapi:v1.0"
     }
	
	stage("User Acceptance Test - AuthService"){
		
		def response = input message: 'Is this build good to go?',
		parameters: [choice(choices: 'Yes\nNo',
		description: '', name: 'Pass')]
		
		if (response=="Yes") {
		stage('Deploy to Kubernetes cluster -AuthService'){
			sh "kubectl create deployment authapi --image=authapi:v1.0"
			sh "kubectl set env deployment/authapi API_HOST=10.107.63.253:8080"
			sh "kubectl expose deployment authapi --type=LoadBalancer --port=8081"
		}
	  }
	}
	
	stage("Production Deployment View"){
        sh "kubectl get deployments"
        sh "kubectl get pods"
        sh "kubectl get services"
    }
}