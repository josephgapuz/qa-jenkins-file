def COLOR_MAP = ['SUCCESS': 'good', 'FAILURE': 'danger', 'UNSTABLE': 'danger', 'ABORTED': 'danger']

pipeline 
{
  agent any
  
  stages 
  {
    
    stage('Checkout Project') 
    {
      steps 
      {
      	slackSend channel: '#got', 
			color: COLOR_MAP['SUCCESS'],
			message: "*STARTED:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER} More info at: ${env.BUILD_URL}" 
			
        git credentialsId: 'GitHub', url: 'https://github.com/josephgapuz/sandbox.git'
      }
    }
    
    
    stage('Build-Test') 
    {
      steps 
      {
        bat label: '', script: 'mvn clean install'   
        junit allowEmptyResults: true, testResults: '**target/surefire-reports/*.xml'
      }
    } 
    
       
    stage('Run SonarQube') 
    {
      steps 
      {
        bat label: '', script: 'sonar-scanner'
      }
    }
    
    
    stage('Deploy Release Artifact to Archiva') 
    {
      steps 
      {
        bat label: '', script: 'mvn release:prepare release:perform'
        archiveArtifacts(artifacts: 'target/*.war', fingerprint: true)
      }
    } 
    
    
    stage('Deploy To Tomcat') 
    {
      steps 
      {
        build job: 'GOT-Deploy-to-Test'
      }
    }
    
    
  }
  
  post 
  {
        always 
        {
            echo 'Sending email notification!'
            
            emailext body: "${currentBuild.currentResult}: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n More info at: ${env.BUILD_URL}",
                recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']],
                subject: "Jenkins Build ${currentBuild.currentResult}: Job ${env.JOB_NAME}"
          
        
          
            echo 'Sending slack notification!'
            
            slackSend channel: '#got',
                color: COLOR_MAP[currentBuild.currentResult],
                message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER} More info at: ${env.BUILD_URL}"
            
        }
  }
  
}
