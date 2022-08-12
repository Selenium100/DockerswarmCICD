pipeline{
	
	agent any
	
	

	stages{
		
		stage('Setup parameters') {
            steps {
                script { 
                    properties([
                        parameters([
                            choice(
                                choices: ['master', 'testing'], 
                                name: 'Branch'
                            ),
                            booleanParam(
                                defaultValue: true, 
                                description: '', 
                                name: 'CHECK_ON_SUCCESS'
                            ),
                            
                            string(
                                defaultValue: 'scriptcrunch', 
                                name: 'STRING-PARAMETER', 
                                trim: true
                            )
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
			sh "ssh -o StrictHostKeyChecking=no  ec2-user@172.31.95.120 'docker container rm -f cloudcontainer'"
			sh "ssh -o StrictHostKeyChecking=no  ec2-user@172.31.95.120 'docker image rmi nityarinky100/declarative-pipeline:latest'"
			sh "ssh -o StrictHostKeyChecking=no  ec2-user@172.31.95.120 'docker container run -p 9090:80 -d --name cloudcontainer nityarinky100/declarative-pipeline:latest'"
			 
}

		 
		 
         }
	}
	}
}
