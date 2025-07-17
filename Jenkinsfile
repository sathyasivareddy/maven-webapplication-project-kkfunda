pipeline {
   agent any
   tools
   {
      maven-3.9.9
   }
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
