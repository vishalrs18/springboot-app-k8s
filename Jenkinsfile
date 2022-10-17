pipeline{
      tools {
        maven 'Maven3'
    }
    agent any
     environment{
        registry = "318041867722.dkr.ecr.us-east-1.amazonaws.com/ekscluster123"
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
            sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 318041867722.dkr.ecr.us-east-1.amazonaws.com'
            sh 'docker push 318041867722.dkr.ecr.us-east-1.amazonaws.com/ekscluster123:latest'
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

  stage ('Slack Notification for QA Deploy') {
      steps {
        slackSend channel: 'C046RTGD1MG,devops-ci-cd-pipeline', message: 'Deployment has been done successfully'
      }
    }  
  }
}
