node{
   stage('SCM Checkout'){
     git 'https://github.com/kris0693/my-app.git'
   }
   stage('Compile-Package'){

      def mvnHome =  tool name: 'maven3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'
   }
   stage('SonarQube Analysis') {
	        def mvnHome =  tool name: 'maven3', type: 'maven'
	        withSonarQubeEnv('sonar') { 
	          sh "${mvnHome}/bin/mvn sonar:sonar"
	        }
	    }
   stage('Build Docker Imager'){
   sh 'docker build -t kris0693/myweb:0.0.2 .'
   }
   stage('Docker Image Push'){
   withCredentials([string(credentialsId: 'dockerhot', variable: 'dockerpassword')]) {
   sh "docker login -u kris0693 -p ${dockerpassword}"
    }
   sh 'docker push kris0693/myweb:0.0.2'
   }
   stage('Nexus Image Push'){
   sh "docker login -u admin -p Admin123 3.8.138.32:8082"
   sh "docker tag kris0693/myweb:0.0.2 3.8.138.32:8082/kris:1.0.0"
   sh 'docker push 3.8.138.32:8082/kris:1.0.0'
   }
   stage('Remove Previous Container'){
	try{
		sh 'docker rm -f tomcattest'
	}catch(error){
		//  do nothing if there is an exception
	}
   stage('Docker deployment'){
   sh 'docker run -d -p 8090:8080 --name tomcattest kris0693/myweb:0.0.2' 
   }
}
}
#docker
#sonar
#sonar1
#oiu
#lol
