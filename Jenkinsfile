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
          /* withSonarQubeEnv("My SonarQube Server") {
           sh "${tool("sonarqube")}/bin/sonar-scanner \*/
  def sonarScanner = tool name: 'SonarQube', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
              sh "${sonarScanner}/bin/sonar-scanner -e -Dsonar.host.url=http://54.225.239.136:9000,-Dsonar.projectKey = multibranch,-Dsonar.sources=maven-selenium-webdriver-testng-example-project",-Dsonar.login=25f5fbbc5c62654b506e24049302da0aaa34370e /
      
           }
 
       
   }
  
               


