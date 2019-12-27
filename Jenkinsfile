node{
 
  //echo "GitHub BranhName ${env.BRANCH_NAME}"
  //echo "Jenkins Job Number ${env.BUILD_NUMBER}"
  echo "Jenkins Node Name ${env.NODE_NAME}"
  
  echo "Jenkins Home ${env.JENKINS_HOME}"
  echo "Jenkins URL ${env.JENKINS_URL}"
  echo "JOB Name ${env.JOB_NAME}"
  
    
    properties([
    buildDiscarder(logRotator(numToKeepStr: '3')),
    pipelineTriggers([
        pollSCM('* * * * *')
    ])
  ])
    
    MavenPath=tool name :'maven3.6.3'
    
    stage('CheckoutGit'){
    git branch: 'development', credentialsId: '45f3eb31-4c46-4055-bcc5-34604504f057', url: 'https://github.com/mpp-DevOpsTest/maven-web-application.git'
    }
    stage('Build'){
    sh "${MavenPath}/bin/mvn clean package"
    }
    stage('SonarQubeTests'){
    sh "${MavenPath}/bin/mvn sonar:sonar"
    }
    stage('CreatingArtifacts'){
    sh "${MavenPath}/bin/mvn clean deploy"  
    }
    stage('DeployToContainer'){
        deploy adapters: [tomcat9(credentialsId: 'b4e3cd44-ec31-4917-9c9d-a776c63fd584', path: '', url: 'http://13.233.13.86:8080/')], contextPath: null, war: '**/*.war'
    }
    stage('EmailNotification'){
        emailext body: 'Finished running the pipeline', subject: 'Build Result', to: 'harshavjpm@gmail.com'
    }
}
