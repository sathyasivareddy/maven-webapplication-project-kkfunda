pipeline {
   agent any
   tools
   {
      maven "maven-3.9.9"
   }
parameters {
        gitParameter(
            name: 'development',
            type: 'PT_BRANCH',
           // branchFilter: 'origin/(.*)', // Filter all remote branches
            defaultValue: 'development',        // Default branch
            description: 'development branch to build',
            sortMode: 'DESCENDING',      // Sort branches by recently updated
            useRepository: 'https://github.com/sathyasivareddy/maven-webapplication-project-kkfunda/' // Your repo URL
        )
        
        // Optional: Add other parameters
        choice(
            name: 'BUILD_TYPE',
            choices: ['debug', 'release'],
            description: 'Select build type'
        )
    }

   
triggers {
   //pollSCM('* * * * *')
   githubPush()

}
      
   stages{
      stage('git checkout') {
         steps{
            //git branch: 'development', url: 'https://github.com/sathyasivareddy/maven-webapplication-project-kkfunda/'
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
            sh """
    curl -u kk:password \
    --upload-file /var/lib/jenkins/workspace/jio-declarative-pipeline/target/maven-web-application.war \
    "http://54.86.40.225:8080/manager/text/deploy?path=/maven-web-applicaton&update=true"
    """
    }
         }
      
      } // stages end 
   } // pipeline end 
