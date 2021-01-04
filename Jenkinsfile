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
 stage('SonarQube Analysis'){
  def mavenHome = tool name: "Maven-3.6.3",type: "maven"
  withSonarQubeEnv('sonar-6') {
  sh "${mavenHome}/bin/mvn/ sonar:sonar"
 }
 }
 stage('publish docker'){
       sh  'docker build -t tejasrik/devopspipeline .'
       sh 'docker login -u tejasrik -p Tejasri@6523'
       sh'docker push tejasrik/devopspipeline'
       sh'docker run -d tejasrik/devopspipeline'
    }	
 stage('Code Quality Check via SonarQube') {
      
       def scannerHome = tool 'sonarqube';
           withSonarQubeEnv("My SonarQube Server") {
            def sonarscanner = tool name:"SonarScanner 4.0",type: "sonarscanner"
           sh "${tool("sonarqube")}/bin/sonar-scanner \
          -Dsonar.projectKey = multibranch \
           -Dsonar.sources=maven-selenium-webdriver-testng-example-project \
           -Dsonar.login=25f5fbbc5c62654b506e24049302da0aaa34370e"
      
           }
 
       
   }
  
               

}
