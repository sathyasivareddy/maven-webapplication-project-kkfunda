pipeline {
   agent any
   stages{
      stage('git checkout') {
         steps{
            git branch: 'development', url: 'https://github.com/sathyasivareddy/maven-webapplication-project-kkfunda/'
         }
      }
      stage('BUILD') {
         steps{
            sh "${mavenHome}/bin/mvn clean package"
         }
      }
      } // stages end 
   } // pipeline end 
