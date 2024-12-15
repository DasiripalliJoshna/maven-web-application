node{

properties([pipelineTriggers([pollSCM('* * * * *')])])
def mavenHome= tool name: 'maven3.9.9'

stage('checkoutcode'){
git branch: 'development', credentialsId: '24d05690-2a44-4171-8a96-92709ed48b51', url: 'https://github.com/DasiripalliJoshna/maven-web-application.git'
}

stage('Build'){
sh "${mavenHome}/bin/mvn clean package"

}
stage('sonarqube'){
sh "${mavenHome}/bin/mvn clean sonar:sonar"

}
stage('nexus'){
sh "${mavenHome}/bin/mvn clean deploy"
}
stage('deploy to container'){
sshagent(['d30823dc-48e5-4a4a-b481-3a18ce3f1a30']) 
sh "scp -o StrictHostKeyChecking=no $WORKSPACE/target/maven-web-application.war ec2-user@ip-172-31-32-121:/opt/apache-tomcat-9.0.97/webapps/"
}

}
