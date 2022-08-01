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
    
    stage ('Check-Git-Secrets') {
      steps {
        sh 'rm trufflehog || true'
        sh 'docker run gesellix/trufflehog --json https://github.com/shashwatsharm/webapp.git > trufflehog'
        sh 'cat trufflehog'
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
         sh 'scp -i /var/lib/jenkins/jenkins2.pem -o StrictHostKeyChecking=no target/*.war ubuntu@65.0.102.150:/home/ubuntu/prod/apache-tomcat-10.0.22/webapps/webapp.war'
      }
    }
  }
  }
}
