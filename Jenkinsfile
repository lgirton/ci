pipeline {
  agent {
    label 'maven'
  }
  
  stages {    
    
    stage('Checkout') {
      steps {
        git url: "${SOURCE_REPO}", branch: "${SOURCE_REF}", credentialsId: "build-github"
      }
    } 
    
    stage('Test') {
      steps {
        sh "mvn -B test"
      }
    }
    
    stage('Build') {
      steps {
        sh "mvn -DskipTests package"   
      }
    }
    
  }
  
}
