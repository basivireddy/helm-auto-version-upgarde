#!/usr/bin/env groovy

VERSION = 'unknown'

def loadValuesYaml(x){
  def valueYaml = readyaml (file: 'charts/hello-world/Chart.yaml')
  return  valueYaml[x];
}

pipeline {
  agent any
  environment {
    HELM_VERSION = loadValuesYaml('version')
  }
  stages {
    stage('test'){
      steps{
         sh "echo ${HELM_VERSION}"
      }
    }
    
  }
  
}
