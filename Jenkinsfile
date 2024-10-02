pipeline {
      agent any
      stages {
        stage ('Docker Image for postgresql') {
          steps {
             sh '''docker run -e POSTGRES_USER=petclinic -e POSTGRES_PASSWORD=petclinic -e POSTGRES_DB=petclinic -p 5432:5432 postgres:17.0 
                  docker-compose --profile postgres up
                  '''
             }
           }
        stage('maven Build'){
          steps{
            sh './mvnw package'
          }
        }
         stage ('Docker Image Build') {
          steps {
             sh '''cd $WORKSPACE
                   docker build -t pet-clinic:v${BUILD_NUMBER} .'''
             }
           }
           stage('Push Image to ECR'){
            steps {
                sh '''aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 559050209565.dkr.ecr.us-east-1.amazonaws.com
           		    
			                 docker tag pet-clinic:v${BUILD_NUMBER} 559050209565.dkr.ecr.us-east-1.amazonaws.com/pet-clinic:v${BUILD_NUMBER}
                    	 docker 559050209565.dkr.ecr.us-east-1.amazonaws.com/pet-clinic:v${BUILD_NUMBER}
		   
       '''
            }
          }
      //  stage('deploy into k8'){
       //   steps{
       //        sh''' kubectl apply petclinic.yaml
       //              kubectl apply petsvc.yaml
       //           '''
       //   }      
       // }
        }
}
