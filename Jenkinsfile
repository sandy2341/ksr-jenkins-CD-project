pipeline {

  environment {
    app = "webapp"
    environment = "prod"
    namespace = "prod"
  }
  agent any

    stages {

      stage ('Checkout SCM'){
        steps {
          checkout([$class: 'GitSCM', branches: [[name: '*/prod']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'git', url: 'https://dptrealtime@bitbucket.org/dptrealtime/webapp-cicd-deployments.git']]])
        }
      }
	  
	  stage ('Deploy Helm Charts')  {
	      steps {
         
           dir("charts"){
             withCredentials([usernamePassword(credentialsId: 'jfrog', usernameVariable: 'username', passwordVariable: 'password')]) {
                    sh 'sudo /usr/local/bin/helm repo add hello-helm-local  https://dptdemo.jfrog.io/artifactory/sts-helm-local --username $username --password $password'
                    sh "sudo /usr/local/bin/helm repo update"
                    sh "sudo /usr/local/bin/helm upgrade ${app}-${environment} --install --namespace ${namespace} --force -f values.yaml ."
                    sh "sudo /usr/local/bin/helm list -a --namespace ${namespace}"
                }
           }
        }
         
      }
         
   } 
}