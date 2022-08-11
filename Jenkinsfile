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
		 
		 withCredentials([string(credentialsId: '', variable: 'dockerhubpassword')]) {
    // some block
			 sh 'docker login -u nityarinky100 -p ${dockerhubpassword}'
			 sh 'docker image push $JOB_NAME.v1.$BUILD_ID'
			 sh 'docker image push nityarinky100/$JOB_NAME.v1.$BUILD_ID'
			 sh 'docker image push nityarinky100/$JOB_NAME:latest'
			 sh 'docker image rmi $JOB_NAME.v1.$BUILD_ID nityarinky100/$JOB_NAME.v1.$BUILD_ID nityarinky100/$JOB_NAME:latest'
			 echo 'Docker image is pushed to dockerhub'
}

		 
		 
         }
	}
	}
}
