#!/usr/bin/env groovy

VERSION = 'unknown'

def loadValuesYaml(x){
  def valueYaml = readyaml (file: 'charts/hello-world/Chart.yaml')
  return  valueYaml[x];
}

pipeline {
  environment {
    HELM_VERSION = loadValuesYaml('version')
  }
  stages {
    stage('test'){
      sh "echo ${HELM_VERSION}"
    }
    
  }
  
}
