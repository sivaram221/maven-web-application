pipeline{
    
    agent any
    
    /*
    agent{
    label "nodename"    
    }
    */
    
    tools{
        maven "maven3.8.2"
    }
    
    options{
        timestamps()
        buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5'))
    }
    
    triggers{
        //pollSCM
    pollSCM('* * * * *')
    //Build Periodically
    cron('* * * * *')
    //GitHub Webhook
    githubPush()
    }
    
    stages{
        
        stage('checkoutcode')
        {
             steps{
            git branch: 'development', credentialsId: '4db79855-fdae-46f4-8673-c807689b0f3d', url: 'https://github.com/sivaram221/maven-web-application.git'    
            }
        }
        stage('build'){
            steps{
                sh "mvn clean package"
            }
        }
        
        stage('executesonarqubereport'){
            steps{
                sh "mvn clean sonar:sonar"
            }
        }
        
        stage('uploadartifactsintonexus'){
            steps{
                sh "mvn clean deploy"
            }
        }
        
        stage('deployapplicationintotomcat'){
            steps{
                sshagent(['b7277e6d-c87b-4454-b778-106874360033']) {
                sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.233.199.52:/opt/apache-tomcat-9.0.52/webapps"
            
                
                }
        }
    }
}// stages closing

post{
    always{
     emailext body: '''Build is completed..!!

Regards,
Siva Ram''', subject: 'Build over..!!', to: 'sivaram.0341@gmail.com'
   
    }
    failure{
        emailext body: '''Build is completed - failed!!

Regards,
Siva Ram''', subject: 'Build over..!!', to: 'sivaram.0341@gmail.com'

         }
         
         success{
             emailext body: '''Build is completed - success!!

Regards,
Siva Ram''', subject: 'Build over..!!', to: 'sivaram.0341@gmail.com'

             
         }
         
}

}//pipeline closing

                
 
