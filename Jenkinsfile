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
    
    stage('Build') {
      steps {
        sh "mvn -B -DskipTests package"
      }
    }
    
    stage('Test') {
      steps {
        sh "mvn -B test"   
      }
    }
    
    stage('Build Image') {
      steps {
        script {
          withCluster() {
            openshift.selector("bc", "${APP_NAME}").startBuild("--from-dir=. --build-loglevel=5").logs("-f")
          }
        }
      }
      
    }
    
  }
  
}
