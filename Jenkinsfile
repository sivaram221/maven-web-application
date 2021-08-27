node{ 

def mavenHome = tool name: "maven3.8.2"

stage('CheckOutCode')
{
git branch: 'development', credentialsId: '4db79855-fdae-46f4-8673-c807689b0f3d', url: 'https://github.com/sivaram221/maven-web-application.git'
}
stage('Build')
{
 sh "${mavenHome}/bin/mvn clean package"
}

stage('sonarqubereport'){
sh "${mavenHome}/bin/mvn clean sonar:sonar"    
}
 
 

stage('uploadartifactintonexus'){
sh "${mavenHome}/bin/mvn clean deploy "
}

stage('deployappintotomcatserver'){
sshagent(['b7277e6d-c87b-4454-b778-106874360033']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@65.0.135.13:/opt/apache-tomcat-9.0.52/webapps"
}    

}



stage('sendemailnotification'){
emailext body: '''Build is completed..!!

Regards,
Siva Ram''', subject: 'Build over..!!', to: 'sivaram.0341@gmail.com'
}

}
