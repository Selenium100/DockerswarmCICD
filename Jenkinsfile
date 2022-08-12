pipeline{
	
	agent any

	stages{

	stage("Pull code from Github"){
         
         steps{

             git 'https://github.com/Selenium100/DockerswarmCICD.git'
           echo 'Code is pulled'
         }
	}
		
		stage("Build Docker image"){
         
         steps{

		 sh 'docker image build -t $JOB_NAME.v1.$BUILD_ID .'
		 sh 'docker image tag $JOB_NAME.v1.$BUILD_ID nityarinky100/$JOB_NAME.v1.$BUILD_ID'
		 sh 'docker image tag $JOB_NAME.v1.$BUILD_ID nityarinky100/$JOB_NAME:latest'
		 echo 'Docker image build is successful'
		 
         }
	}
		
		stage("Push Docker image to DockerHub"){
         
         steps{
		 
		withCredentials([string(credentialsId: 'nityapass3', variable: 'nityapass3')]) { 
    // some block
			 sh 'docker login -u nityarinky100 -p ${nityapass3}'
			 sh 'docker image push nityarinky100/$JOB_NAME.v1.$BUILD_ID'
			 sh 'docker image push nityarinky100/$JOB_NAME:latest'
			 sh 'docker image rmi $JOB_NAME.v1.$BUILD_ID nityarinky100/$JOB_NAME.v1.$BUILD_ID nityarinky100/$JOB_NAME:latest'
			 echo 'Docker image is pushed to dockerhub'
}

		 
		 
         }
	}
		
		
		stage("Pull latest image from Dockerhub and deploy container"){
         
         steps{
		 
		 def dockerrun = 'docker container run -p 9090:80 -d --name cloudcontainer nityarinky100/declarative-pipeline:latest'
		sshagent(['dockerhubpass2']) {
    // some block
			
			sh "${dockerrun}"
}

		 
		 
         }
	}
	}
}
