
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
    stage ('Source Composition Analysis') {
      steps {
         sh 'rm owasp* || true'
         sh 'wget "https://<URL to script>/owasp-dependency-check.sh" '
         sh 'chmod +x owasp-dependency-check.sh'
         sh 'bash owasp-dependency-check.sh'
         sh 'cat /var/lib/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.xml'

      }
    }
    stage ('Tomcat-Deploy') {
      steps {
        sshagent(['tomcat_server']) {
          sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@54.176.139.137:/opt/tomcat/webapps/webapp.war'
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
