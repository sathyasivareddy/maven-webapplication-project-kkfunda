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
    }
    
    post {
        always {
            // Clean workspace after build
            cleanWs()
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
