pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', credentialsId: 'harigithub', url: 'https://github.com/harsha0620/lms'
            }
        }
         stage('maven build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('dev deploy') {
            steps {
                sshagent(['tomcat-dev']) {
                    
                 // copy war file to dev tomcat
                 sh "scp target/lms.war ec2-user@172.31.22.251:/opt/tomcat9/webapps"
                 
                 // stop tomcat
                 sh "ssh ec2-user@172.31.22.251 /opt/tomcat9/bin/shutdown.sh"
                 
                 // start tomcat
                 sh "ssh ec2-user@172.31.22.251 /opt/tomcat9/bin/startup.sh"
               }
            }
        }
    }
}
