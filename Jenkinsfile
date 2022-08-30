node {
	stage("Checkout - AuthService"){
		git branch: 'main', url: 'https://github.com/dipendrashrestha1281/msd-authservice'
	}
	
	stage("Gradle Build - AuthService"){
		sh 'gradle clean build'
	}
	
	stage ("Gradle Bootjar-Package - AuthService") {
        sh 'gradle bootjar'
    }
    
    stage ("Containerize the app-docker build - AuthService") {
        sh 'docker build --rm -t test-authapi:v1.0 .'
    }
    
    stage ("Inspect the docker image - AuthService"){
        sh "docker images test-authapi:v1.0"
        sh "docker inspect test-authapi:v1.0"
    }
    
    stage ("Run Docker container instance - AuthService"){
        sh "docker run -d --rm --name testauthapi -p 8081:8081 test-authapi:v1.0"
     }
	
	stage("User Acceptance Test - AuthService"){
		
		def response = input message: 'Is this build good to go?',
		parameters: [choice(choices: 'Yes\nNo',
		description: '', name: 'Pass')]
		
		if (response=="Yes") {
		stage('Deploy to Kubernetes cluster -AuthService'){
			sh "kubectl create deployment test-project-auth --image=test-authapi:v1.0"
			sh "kubectl set env deployment/test-project-auth API_HOST=10.96.221.247:8080"
			sh "kubectl expose deployment test-project-auth --type=LoadBalancer --port=8081"
		}
	  }
	}
	
	stage("Production Deployment View"){
        sh "kubectl get deployments"
        sh "kubectl get pods"
        sh "kubectl get services"
    }
}
