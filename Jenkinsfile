pipeline {
   agent any
   tools
   {
      maven "maven-3.9.9"
   }
parameters {
        gitParameter(
            name: 'TARGET_BRANCH',
            type: 'PT_BRANCH',
            branchFilter: '/(.*)', // Filter all remote branches
            defaultValue: 'development',        // Default branch
            description: 'development branch to build',
            sortMode: 'DESCENDING',      // Sort branches by recently updated
            useRepository: 'https://github.com/sathyasivareddy/maven-webapplication-project-kkfunda/' // Your repo URL
        )
        
        // Optional: Add other parameters
        choice(
            name: 'BUILD_TYPE',
            choices: ['debug', 'release'],
            description: 'development branch '
        )
    }

   
triggers {
   //pollSCM('* * * * *')
   githubPush()

}
      
   stages{
      stage('git checkout') {
         steps {
            script {
               // Clean branch name by removing 'origin/' if present
               def branchName = params.SELECTED_BRANCH.replaceFirst()
               
               checkout([
                  $class: 'GitSCM',
                  branches: [[name: branchName]],
                  userRemoteConfigs: [[
                     url: 'https://github.com/sathyasivareddy/maven-webapplication-project-kkfunda/',
                     //credentialsId: 'your-github-credentials' // Add your credentials ID here
                  ]]
               ])
            }
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
