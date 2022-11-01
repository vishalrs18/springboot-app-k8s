pipeline{
      tools {
        maven "maven-1"
    }
    agent any
     environment{
        registry = "802613590849.dkr.ecr.ap-northeast-1.amazonaws.com/demo1"
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
         sudo docker.build registry 
        }
      }
    }
     stage('Push into ECR'){
        steps{
            sh 'aws ecr get-login-password --region ap-northeast-1 | docker login --username AWS --password-stdin 802613590849.dkr.ecr.ap-northeast-1.amazonaws.com'
            sh 'docker push 802613590849.dkr.ecr.ap-northeast-1.amazonaws.com/demo1:latest'
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
