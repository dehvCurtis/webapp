
pipeline {
  agent any
  tools {
    maven 'Maven'
  }
  stages {
    stage ('Initialize') {
      steps {
        sh '''
          echo "PATH = ${PATH}"
          echo "M2_HOME = ${M2_HOME}"
        '''
      }
    }
    stage ('Secret-Scanner') {
        steps {
          sh 'docker pull gesellix/trufflehog'
          sh 'docker run -t gesellix/trufflehog --json https://github.com/dehvCurtis/WebApp.git > truffle_output.json'
          sh 'cat truffle_output.json'
          }
        }
    stage ('Tomcat-Deploy') {
      steps {
        sshagent(['tomcat_server']) {
          sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@<ip_address>:/opt/tomcat/webapps/webapp.war'
        }
      }
    }
    stage ('Build') {
      steps {
        sh 'mvn clean package'
      }
    }
  }
}
