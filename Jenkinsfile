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
    stage ('Build') {
      steps {
        sh 'mvn clean package'
      }
    }
    stage ('Tomcat-Deploy') {
      steps {
        sshagent(['tomcat_server']) {
          sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@13.52.102.33:/opt/tomcat/webapps/webapp.war'
        }
      }
    }
    stage ('Secret-Scanner') {
        steps {
          sh 'docker pull gesellix/trufflehog'
          sh 'docker run -t gesellix/trufflehog --json https://github.com/dehvCurtis/WebApp.git > truffle_output.json'
          }
        }
  }
}
