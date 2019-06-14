pipeline {
  agent any
  stages {
    stage('Download Artifact') {
      steps {
        bat label: '', script: 'curl http://localhost:9090/repository/snapshots/com/sample/sandbox/1.0-SNAPSHOT/sandbox-1.0-20190606.202852-1.war --output got.war'
      }
    }
    stage('Deploy To QA Tomcat') {
      steps {
        build job: 'GOT-Deploy-to-Test'
      }
    }   
  }
}