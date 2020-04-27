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

   stage('Deploy into K8s Clusters'){
        withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'aws-kube', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
        sh 'aws sts get-caller-identity'
        sh 'aws eks --region us-west-2 update-kubeconfig --name k8s-dev-cluster'
        sh 'kubectl apply -f manifest.yaml'
        sh 'kubectl rollout restart deployment/webapp'
      }
    }
}

