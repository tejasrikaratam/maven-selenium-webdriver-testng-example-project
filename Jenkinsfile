@Library('shared-library@master') _

node {
stage('SCM Checkout')
{
 
 gitCheckout(
branch: "master",
url: "https://github.com/tejasrik/e2e.git"
)
}

 stage('Compile-Package'){
      // Get maven home path
     //def mvnHome =  tool name: 'maven3.6.3', type: 'maven' 
     //batlabel "${mvnHome}/bin/mvn package"
  // bat "${mvnHome}/bin/gmvn clean package"
 // bat label: '', script: "${mvnHome}/bin/mvn clean package"
         def mavenHome = tool name:"Maven-3.6.3",type: "maven"
         
         def mavenCMD= "${mavenHome}/bin/mvn"
         sh "${mavenCMD} clean package"
  
}
  stage('SonarQube analysis') {
    def scannerHome = tool 'SonarScanner 4.0';
    withSonarQubeEnv('My SonarQube Server') { // If you have configured more than one global server connection, you can specify its name
      sh "${scannerHome}/bin/sonar-scanner"
    }
  }
                stage('Stop Tomcat') {
                sh "ssh -T 'root@${server}' /opt/apache-tomcat-8.5.31/bin/./shutdown.sh"
                }
        
                stage('War File Deployment') {
                sh "ssh -T 'root@${server}' rm -f /opt/apache-tomcat-8.5.31/webapps/petclinic.war rm -fr /opt/apache-tomcat-8.5.31/webapps/petclinic"
                sh "scp target/petclinic.war 'root@${server}':/opt/apache-tomcat-8.5.31/webapps/"
                }
                stage('Start Tomcat') {
                sh "ssh -T 'root@${server}' /opt/apache-tomcat-8.5.31/bin/./startup.sh"
                }
        notify('Sucess')
        
}
