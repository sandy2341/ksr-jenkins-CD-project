pipeline {

  environment {
    app = "webapp"
    environment = "master"
    namespace = "master"
  }
  agent any

    stages {

      stage ('Checkout SCM'){
        steps {
          checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'git', url: 'https://github.com/sandy2341/ksr-jenkins-CD-project.git']]])
        }
      }
	  
	  stage ('Deploy Helm Charts')  {
	      steps {
         
           dir("charts"){
             withCredentials([usernamePassword(credentialsId: 'JFrog', usernameVariable: 'username', passwordVariable: 'password')]) {
                    sh 'sudo /usr/local/bin/helm repo add ksr-helm-local  https://sandya.jfrog.io/artifactory/ksr-helm-local --username $username --password $password'
                    sh "sudo /usr/local/bin/helm repo update"
                    sh "sudo /usr/local/bin/helm upgrade ${app}-${environment} --install --namespace ${namespace} --force -f values.yaml ."
                    sh "sudo /usr/local/bin/helm list -a --namespace ${namespace}"
                }
           }
        }
         
      }
         
   } 
}
