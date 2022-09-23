#!/usr/bin/env groovy

VERSION = 'unknown'

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
         sh'''
           echo "current version is ${HELM_VERSION}"
           HELM_VERSION=`echo "${HELM_VERSION}" | awk -F. '{$NF = $NF + 1;} 1' | sed 's/ /./g'`
           echo "updated version is ${HELM_VERSION}"
         '''
      }
    }
     
    stage('final'){
      steps{
          sh "echo ${HELM_VERSION}"
      }
    }    
  
  
  }
}
