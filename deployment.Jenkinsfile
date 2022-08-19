pipeline{
	
	agent any
	
	options{
	timestamps()
	}
	
	

	stages{
		
		stage('Setup parameters') {
            steps {
                script { 
                    properties([
                        parameters([
                            string(
                                name: 'Branch',
				defaultValue: 'master' ,
				description: 'Git Branch to checkout'),

				
                            
                            
                        ])
                    ])
                }
            }
        }

	stage("Pull code from Github"){
         
         steps{

             git 'https://github.com/Selenium100/DockerswarmCICD.git'
           echo 'Code is pulled'
         }
	}
		
		stage("Build Docker image"){
         
         steps{
                 sh 'sudo chmod 777 /var/run/docker.sock'
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
                          sh 'sudo chmod 777 /var/run/docker.sock'
			 sh 'docker login -u nityarinky100 -p ${nityapass3}'
			 sh 'docker image push nityarinky100/$JOB_NAME:latest'
			 sh 'docker image rmi $JOB_NAME.v1.$BUILD_ID nityarinky100/$JOB_NAME.v1.$BUILD_ID nityarinky100/$JOB_NAME:latest'
			 echo 'Docker image is pushed to dockerhub'
}

		 
		 
         }
	}
		
		
		stage("Pull latest image from Dockerhub and deploy container"){
         
         steps{
		 
		
		sshagent(['nityadockerhubpass']) {
    // some block       
                         sh 'sudo chmod 777 /var/run/docker.sock'
			sh "ssh -o StrictHostKeyChecking=no  ec2-user@172.31.95.120 'docker container rm -f cloudcontainer'"
			sh "ssh -o StrictHostKeyChecking=no  ec2-user@172.31.95.120 'docker image rmi nityarinky100/declarative-pipeline:latest'"
			sh "ssh -o StrictHostKeyChecking=no  ec2-user@172.31.95.120 'docker container run -p 9090:80 -d --name cloudcontainer nityarinky100/declarative-pipeline:latest'"
			 
}

		 
		 
         }
	}
	}
}
