pipeline{
      tools {
        maven 'Maven3'
    }
    agent any
     environment{
        registry = "public.ecr.aws/v0r4v2b6/docker-repo2907"
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

}

  }
}
