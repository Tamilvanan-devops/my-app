node{
   stage('SCM Checkout'){
     git 'https://github.com/Tamilvanan-devops/my-app.git'
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
   sh 'docker build -t tamilvananv/myweb1:0.0.2 .'
   }
   stage('Docker Image Push'){
   withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
   sh "docker login -u tamilvananv -p ${dockerPassword}"
    }
   sh 'docker push tamilvananv/myweb1:0.0.2'
   }
stage('Docker deployment'){
   sh 'docker run -d -p 8090:8080 --name tomcattest2 tamilvananv/myweb1:0.0.2' 
   }
}
