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
        sh """
           scp -o StrictHostKeyChecking=no target/*.war ubuntu@xxxx:/home/ubuntu
           ssh -o StrictHostKeyChecking=no ubuntu@xxxx 'cp -r /home/ubuntu/*.war /opt/tomcat/latest/webapps/'
         """
        // sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@65.0.102.150:/home/ubuntu/prod/apache-tomcat-10.0.22/webapps/webapp.war'
      }
    }
  }
  }
}
