pipeline {
   agent any
   tools
   {
      maven "maven-3.9.9"
   }
   stages{
      stage('git checkout') {
         steps{
            git branch: 'development', url: 'https://github.com/sathyasivareddy/maven-webapplication-project-kkfunda/'
         }
      }
      stage('BUILD') {
         steps{
            sh "mvn clean package"
         }
      }
      stage('SQ Report')
      {
         steps{
            sh "mvn sonar:sonar"
         }
      }
      stage ('backup artifactory to Nexus')
      {
         steps{
            sh "mvn clean install"
         }
      }
      stage ('deploy to Tomcat ')
      {
         steps {
            sh "mvn clean deploy"
         }
      }
      } // stages end 
   } // pipeline end 
