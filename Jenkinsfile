#!/usr/bin/env groovy

def loadValuesYaml(x){
  def valueYaml = readYaml (file: 'charts/hello-world/Chart.yaml')
  return  valueYaml[x];
}

pipeline {
  agent any
  environment {
    HELM_VERSION = loadValuesYaml('version')
  }
  stages {
    stage('init'){
      steps{
         sh "echo ${HELM_VERSION}"
      }
    }
    
    stage('auto increment'){
      steps{

          echo "current version is ${HELM_VERSION}"
          script {

	          tag = sh (script: "git describe --tags --abbrev=0 HEAD", returnStdout: true).trim()
                  echo "${tag}"
                  cf = sh (script: "git diff --quiet HEAD ${tag} -- charts/hello-world", returnStatus: true)
                  echo "${cf}"
                  if ( cf != 0 ) {
                     echo "There is a change in helm chart"
                     env.Updated_HELM_VERSION = sh (script: "echo ${HELM_VERSION} | awk -F. '{\$NF = \$NF + 1;} 1' | sed 's/ /./g'", returnStdout: true).trim()
                     echo "${Updated_HELM_VERSION}"
                  }
           }


      }
    }
     
    stage('final'){
      steps{
        sh "echo ${HELM_VERSION},${env.Updated_HELM_VERSION}"
        //global env varibles can't override  with assignment , only can override with withEnv block  
        withEnv(["HELM_VERSION=${env.Updated_HELM_VERSION}"]){
              sh "echo ${HELM_VERSION},${env.Updated_HELM_VERSION} "
         }
      }
    }    
	  stage('updated'){
		  steps{  
		  def read = readYaml (file: 'charts/hello-world/Chart.yaml')
		  echo "${read}"
		  assert read.size = ${env.Updated_HELM_VERSION}
		  writeYaml file: 'datas.yaml', data: read
		  sh "cat datas.yaml "
		  
		  }
	  }
  }
}
