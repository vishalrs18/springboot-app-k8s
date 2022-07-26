pipeline{
      tools {
        maven 'Maven3'
    }
    agent any
     environment{
        registry = "public.ecr.aws/f1f7r6x7/kubernetes"
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
            sh 'aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/f1f7r6x7'
            sh 'docker push public.ecr.aws/f1f7r6x7/kubernetes:latest'
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
