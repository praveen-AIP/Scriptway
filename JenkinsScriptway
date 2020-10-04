node
{
    properties([[$class: 'JiraProjectProperty'], buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5'))])
  echo "Build number is : ${env.BUILD_NUMBER}"
  def mavenHome = tool name: "maven-3.6.3"
    stage('CheckoutCode')
    {
        git branch: 'development', credentialsId: '74bac532-9fbe-41a0-ad03-4e0fb5afbdcb', url: 'https://github.com/praveen-AIP/maven-web-application.git'
    }
    stage('Build')
    {
        sh "${mavenHome}/bin/mvn clean package"
    }
    stage('ExecuteSonarQubeReport')
    {
        sh "${mavenHome}/bin/mvn sonar:sonar"
    }
    
    stage('UploadArtifactsIntoNexus')
    {
        sh "${mavenHome}/bin/mvn deploy"
    }
    
    stage('DeployApplicationIntoTOmcat')
    {
        sshagent(['TomcatCrdentials']) 
        {
            sh 'scp -o StrictHostKeyChecking=no target/maven-web-application.war praveen.k.polepalli@35.188.44.54:/opt/apache-tomcat-9.0.38/webapps/'
        }
    }
    
    stage('EmailNotification')
    {
        mail bcc: '', body: 'Build is over', cc: '', from: '', replyTo: '', subject: 'Build Status', to: 'polepalli514@gmail.com'
    }
    
        
}
