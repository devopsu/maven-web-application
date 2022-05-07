node()
{
def mavenHome=tool name:"maven 3.8.4"

stage('CheckoutCode')
{
git branch: 'development', credentialsId: 'e9e29005-ea32-41ee-8ba4-31c3b556149c', url: 'https://github.com/devopsu/maven-web-application.git'
}

stage('Build')
{
sh "${mavenHome}/bin/mvn clean package"
}

stage('ExecuteSonarQubeReport')
{
sh "${mavenHome}/bin/mvn clean sonar:sonar"
}
stage('UploadArtifactIntoNexus')
{
sh "${mavenHome}/bin/mvn clean deploy"
}
stage('DeployAppintoTomcat')
{
sshagent(['a6784d4f-1685-4299-986e-047a3ae5ca59']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.235.90.208:/opt/apache-tomcat-9.0.62/webapps/"
}
  }
  stage('SendNotifications')
{
emailext body: '''build over

Regards
Sarika
9999999999''', subject: 'build over', to: 'sarika1809sharma@gmail.com'
}
}

