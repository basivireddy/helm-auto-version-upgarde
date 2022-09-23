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
         // env.var assignment only can do with in script block
         script {
             git diff --quiet HEAD "\$(git describe --tags --abbrev=0 HEAD)" -- helm/hello-world
             cf=$?
            // echo "change flag ${cf}"
            // if [ $cf -ne 0 ]; then
            //     echo "There is a change in helm chart"
             // fi
             env.Updated_HELM_VERSION = sh (script: "echo ${HELM_VERSION} | awk -F. '{\$NF = \$NF + 1;} 1' | sed 's/ /./g'", returnStdout: true).trim()
             echo "${env.Updated_HELM_VERSION}"
         }
        echo "updated version is ${HELM_VERSION},${env.Updated_HELM_VERSION}"
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
  
  
  }
}
