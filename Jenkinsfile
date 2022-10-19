pipeline{
      tools {
        maven 'Maven3'
    }
    agent any
     environment{
        registry = "288513718779.dkr.ecr.ap-south-1.amazonaws.com/eks"
    }
    stages{
     stage('Cloning Git') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', url: 'https://github.com/dinu2907/springboot-app.git']]])     
            }
   }
    stage('Build'){
        steps{
            sh 'mvn clean install'
        }
            }
    stage('Building image') {
      steps{
        script {
         docker.build registry 
        }
      }
    }
     stage('Push into ECR'){
        steps{
            sh 'aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 288513718779.dkr.ecr.ap-south-1.amazonaws.com'
            sh 'docker push 288513718779.dkr.ecr.ap-south-1.amazonaws.com/eks:latest'
        }
    }
    
    stage('K8S Deployment'){
        steps{
   withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'K8S', namespace: '', serverUrl: '') {
    // some block
    sh "kubectl apply -f eks-deploy-k8s.yaml"
    }
 }
}

 
  }
}
