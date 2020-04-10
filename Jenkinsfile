pipeline {
  agent {
    node {
      label 'maven'
    }
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
          openshift.withCluster() {
            openshift.selector("bc", "${APP_NAME}").startBuild("--from-dir=target/ --build-loglevel=5").logs("-f")
            
            // Extract abbreviated git commit id
            def props = readProperties file: "target/classes/git.properties"
            def abbrev = props['git.commit.id.abbrev']
            def tag = """${APP_NAME}:$abbrev"""
            echo tag
            
            
            
          }
        }
      }
      
    }
    
  }
  
}
