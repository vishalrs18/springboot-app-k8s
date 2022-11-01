pipeline{
      tools {
        maven "maven-1"
    }
    agent any
     environment{
        registry = "public.ecr.aws/b2y6t5b2/demo:latest"
    }
    stages{
     stage('Cloning Git') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', url: 'https://github.com/vishalrs18/springboot-app-k8s.git']]])     
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
            sh 'aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/b2y6t5b2'
            sh 'docker push public.ecr.aws/b2y6t5b2/demo:latest'
        }
    }
    
    stage('K8S Deployment'){
        steps{
   withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'K8S', namespace: '', serverUrl: '') {
    // some block
    sh "kubectl delete all --all"
    sh "kubectl apply -f eks-deploy-k8s.yaml"
    }
 }
}

 
  }
}
