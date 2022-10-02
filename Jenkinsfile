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
    
    stage ('Source Composition Analysis') {
      steps {
         sh 'rm owasp* || true'
         sh 'wget "https://github.com/shashwatsharm/webapp/blob/main/owasp-dependency-check.sh" '
         sh 'ls'
         sh 'pwd'
         sh 'whoami'
         sh 'chmod +x owasp-dependency-check.sh'
       //  sh './owasp-dependency-check.sh'
       //  sh 'cat /var/lib/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.xml'
      }
    }
    
    stage('SAST') {
      steps {
        //withSonarQubeEnv('devopssecure') {
          //sh 'mvn sonar:sonar'
          //sh 'cat target/sonar/report-task.txt'
        //}
        sh 'ls'
      }
    }
    
    stage('Build') {
      steps {
        sh 'ls'
      }
    }
    
    stage ('Send War File to Tomcat Server') {
      steps {
      sh 'mvn clean package' 
    }
    }
    
    stage ('DAST') {
    steps {
      sshagent(['tomcat2']) {
         sh 'scp -i /var/lib/jenkins/jenkins2.pem -o StrictHostKeyChecking=no target/*.war ubuntu@13.127.249.3:/home/ubuntu/prod/apache-tomcat-10.0.22/webapps/webapp.war'
      }
    }
  }
  }
}
