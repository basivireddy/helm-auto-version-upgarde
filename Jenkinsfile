#!/usr/bin/env groovy

def loadValuesYaml(x){
  def valueYaml = readYaml (file: 'charts/hello-world/Chart.yaml')
  return  valueYaml[x];
}

def auto(chartpath,HELM_VERSION){

	script {
	          tag = sh (script: "git describe --tags --abbrev=0 HEAD", returnStdout: true).trim()
                  echo "${tag}"
		cf = sh (script: "git diff --quiet HEAD ${tag} -- ${chartpath}", returnStatus: true)
                  echo "${cf}"
                  if ( cf != 0 ) {
                     echo "There is a change in helm chart"
                     env.Updated_HELM_VERSION = sh (script: "echo ${HELM_VERSION} | awk -F. '{\$NF = \$NF + 1;} 1' | sed 's/ /./g'", returnStdout: true).trim()
                     echo "${Updated_HELM_VERSION}"
                  }
	
				  
		 // def read = readYaml (file: 'charts/hello-world/Chart.yaml')
		 // echo "${read}"
				  if ( env.Updated_HELM_VERSION == null ){
					  env.Updated_HELM_VERSION = HELM_VERSION
				  }
				  
		  sh'''
		   sed -i "s/^version: .*/version: $Updated_HELM_VERSION/" ${chartpath}/Chart.yaml
		  '''
		 sh "cat charts/hello-world/Chart.yaml "
		 // Push the code
				  
				  
		//  read.version = "${env.Updated_HELM_VERSION}"
		//  writeYaml file: 'charts/hello-world/Chart.yaml', data: read, overwrite: true
		//  sh "cat charts/hello-world/Chart.yaml "
			  }
	//return Updated_HELM_VERSION
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
		      auto('charts/hello-world',HELM_VERSION)
	      }


      }
    }
     
    stage('final'){
      steps{
        sh "echo ${HELM_VERSION},${env.Updated_HELM_VERSION}"
        //global env varibles can't override  with assignment , only can override with withEnv block  
        withEnv(["HELM_VERSION=${env.Updated_HELM_VERSION}"]){
              sh "echo ${HELM_VERSION},${env.Updated_HELM_VERSION}"
         }
	 
      }
    }    

  }
}
