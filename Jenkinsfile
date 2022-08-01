pipeline{
  agent any
  tools {
    maven '3.6.3'
  }
  stages {
    stage ('Initiliaze') {
      steps {
        sh '''
                   echo "PATH = $(PATH)"
                   echo "M2_HOME = $(M2_HOME)"
             '''      
      }
    }
    stage ('Build') {
      steps {
      sh 'mvn clean package' 
    }
    }
    
    stage ('Send War File to Tomcat Server') {
    steps {
      sshagent(['tomcat2']) {
        // sh """
        //   scp -o StrictHostKeyChecking=no target/*.war ubuntu@65.0.102.150:/home/ubuntu
        //   ssh -o StrictHostKeyChecking=no ubuntu@65.0.102.150 'cp -r /home/ubuntu/*.war /home/ubuntu/prod/apache-tomcat-10.0.22/webapps/'
        // """
         sh 'scp -i /var/lib/jenkins/jenkins2.pem -o StrictHostKeyChecking=no target/*.war ubuntu@65.0.102.150:/home/ubuntu/prod/apache-tomcat-10.0.22/webapps/webapp.war'
      }
    }
  }
  }
}
