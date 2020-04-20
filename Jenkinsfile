node{
   stage('SCM Checkout'){
       git url: 'https://github.com/arunsankar9/aws-demo.git'
   }
   stage('Mvn Package'){
     def mvnHome = tool name: 'maven-3', type: 'maven'
     def mvnCMD = "${mvnHome}/bin/mvn"
     sh "${mvnCMD} clean package"
   }
      stage('Build Docker Image'){
     sh 'docker build -t webapp .'
      }
      
       stage('Dcoker Tag to Reg'){
     sh 'docker tag webapp:latest 107626280130.dkr.ecr.us-west-2.amazonaws.com/webapp:latest'
      }
     
     stage('Push image to ECR'){
 
withDockerRegistry(credentialsId: 'ecr:us-west-2:aws-key', toolName: 'docker', url: 'https://107626280130.dkr.ecr.us-west-2.amazonaws.com/') {
     sh 'docker push 107626280130.dkr.ecr.us-west-2.amazonaws.com/webapp:latest'
}
        
 

   }
}

