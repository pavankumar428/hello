pipeline{

agent any

tools{
maven 'maven3.8.4'

}

triggers{
pollSCM('* * * * *')
}

options{
timestamps()
buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5'))
}

stages{

  stage('CheckOutCode'){
    steps{
    git branch: 'master', url: 'https://github.com/pavankumar428/hello.git'
	
	}
  }
  
  stage('Build'){
  steps{
  sh  "mvn clean package"
  }
  }

 stage('ExecuteSonarQubeReport'){
  steps{
  sh  "mvn clean sonar:sonar"
  }
  }
 
  stage('UploadArtifactsIntoNexus'){
  steps{
  sh  "mvn clean deploy"
  }
  }
 
  stage('DeployAppIntoTomcat'){
  steps{
  sshagent(['b75ddb57-389b-4607-8bbb-47c090ee09b7']) {
    sh label: '', script: 'scp  /var/lib/jenkins/workspace/p1/target/maven-web-application.war  ubuntu@172.31.85.35:/etc/var/lib/denv.war'
  }
  }
  }
  
  
}//Stages Closing

post{

 success{
 emailext to: 'devopstrainingblr@gmail.com,mithuntechnologies@yahoo.com',
          subject: "Pipeline Build is over .. Build # is ..${env.BUILD_NUMBER} and Build status is.. ${currentBuild.result}.",
          body: "Pipeline Build is over .. Build # is ..${env.BUILD_NUMBER} and Build status is.. ${currentBuild.result}.",
          replyTo: 'devopstrainingblr@gmail.com'
 }
 
 
 failure{
 emailext to: 'devopstrainingblr@gmail.com,mithuntechnologies@yahoo.com',
          subject: "Pipeline Build is over .. Build # is ..${env.BUILD_NUMBER} and Build status is.. ${currentBuild.result}.",
          body: "Pipeline Build is over .. Build # is ..${env.BUILD_NUMBER} and Build status is.. ${currentBuild.result}.",
          replyTo: 'devopstrainingblr@gmail.com'
 }
 
}


}//Pipeline closing
