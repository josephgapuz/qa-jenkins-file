pipeline {
  environment {
     buildArtifact = "http://localhost:9090/repository/repo/com/sample/sandbox/${BUILD_VERSION}/sandbox-${BUILD_VERSION}.war"
   }
  agent any
  stages {
    stage('Download Artifact') {
      steps {
        bat label: '', script: 'curl %buildArtifact% --output got.war'
        archiveArtifacts(artifacts: 'got.war', fingerprint: true)
      }
    }
    stage('Deploy To QA Tomcat') {
      steps {
        build job: 'GOT-Deploy-to-Test'
      }
    }   
  }
}
