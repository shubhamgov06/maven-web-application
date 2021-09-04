node
{
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '20')), pipelineTriggers([pollSCM('* * * * *')])])
    def mavenhome= tool name:"maven3.8.2"
    
    stage('codefrom Github')
    {
        git branch: 'development', url: 'https://github.com/shubhamgov06/maven-web-application.git'
    }
    
    stage('Maven code Build')
    {
        sh "${mavenhome}/bin/mvn clean package"
    }
    
    stage('SonarQubeRepo')
    {
        sh "${mavenhome}/bin/mvn clean package sonar:sonar"
    }
    
    
     stage('DeploytoTomcat')
    {sshagent(['75f12830-7e76-4e7d-bc96-110705274e29'])
{
 sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war centos@3.109.206.220:/opt/apache-tomcat-9.0.52/webapps"   
}

    }
    
    stage ('Email Notification')
    {
        emailext body: 'Pipeline Build Status.See below:', subject: 'Pipeline Build Status', to: 'shubh.nis@gmail.com'
    }
}

