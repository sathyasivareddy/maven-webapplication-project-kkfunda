pipeline {
   agent any
   tools
   {
      maven "maven-3.9.9"
   }
parameters {
    gitParameter(
        name: 'SELECTED_BRANCH',
        type: 'PT_BRANCH',
        branchFilter: 'origin/(.*)',
        defaultValue: 'origin/development',
        description: 'Select branch to build',
        sortMode: 'DESCENDING',
        useRepository: 'https://github.com/sathyasivareddy/maven-webapplication-project-kkfunda.git'
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
            // Safely handle branch name with proper cleaning
            def branchName = params.SELECTED_BRANCH.replaceAll('origin/', '')
            
            checkout([
                $class: 'GitSCM',
                branches: [[name: branchName]],
                userRemoteConfigs: [[
                    url: 'https://github.com/sathyasivareddy/maven-webapplication-project-kkfunda.git',
                    credentialsId: 'github-credentials'
                ]],
                extensions: [
                    [
                        $class: 'LocalBranch',
                        localBranch: branchName
                    ]
                ]
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
