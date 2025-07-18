pipeline {
    agent any
    
    tools {
        maven "maven-3.9.9"
    }
    
    triggers {
        // pollSCM('* * * * *')  // Correct cron syntax if needed
        githubPush()
    }
    
    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'development', url: 'https://github.com/sathyasivareddy/maven-webapplication-project-kkfunda/'
            }
        }
        
        stage('BUILD') {
            steps {
                sh "mvn clean package"
            }
        }
        
        stage('SQ Report') {
            steps {
                sh "mvn sonar:sonar"
            }
        }
        
        stage('Backup Artifactory to Nexus') {
            steps {
                sh "mvn clean install"
            }
        }
        
        stage('Deploy to Tomcat') {
            steps {
                sh """
                curl -u kk:password \
                --upload-file /var/lib/jenkins/workspace/jio-declarative-pipeline/target/maven-web-application.war \
                "http://54.86.40.225:8080/manager/text/deploy?path=/maven-web-application&update=true"
                """
            }
        }
        
            stage('idea QA up stream tigger')
            { 
                steps
                {
                build job: 'idea-qa'
            }
            
        }
        
    
    
        post {
        success {
            script {
                notifyBuild(currentBuild.result)
            }
        }

        failure {
            script {
                notifyBuild(currentBuild.result)
            }
        }
    }
}

// Notification method
def notifyBuild(String buildStatus = 'STARTED') {
    buildStatus = buildStatus ?: 'SUCCESS'

    def colorCode
    def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
    def summary = "${subject} (${env.BUILD_URL})"

    switch (buildStatus) {
        case 'STARTED':
            colorCode = '#FFFF00' // Yellow
            break
        case 'SUCCESS':
            colorCode = '#00FF00' // Green
            break
        default:
            colorCode = '#FF0000' // Red
    }

    slackSend(color: colorCode, message: summary)
}
