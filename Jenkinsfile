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
 stage('Code Quality Check via SonarQube') {
      
       def scannerHome = tool 'sonarqube';
           withSonarQubeEnv("sonarqube-container") {
           sh "${tool("sonarqube")}/bin/sonar-scanner"
           -Dsonar.projectKey=jenkins \
           -Dsonar.sources=maven-selenium-webdriver-testng-example-project \
           -Dsonar.host.url=http://54.225.239.136:9000 \
           -Dsonar.login=9a628e5b34d1ad6114d1ae4bbc2768dfd8565b58"
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
