currentBuild.displayName = "online-shopping-#" + currentBuild.number

pipeline {
    agent any

    tools {
        maven 'm1'
    }

    stages {
        stage("Git Checkout") {
            steps {
                git credentialsId: 'github', url: 'https://<PAT>github.com/ShaikhAbubakkar/tomcat-maven-jenkins-pipeline-master.git'
            }
        }
        stage("Maven Build") {
            steps {
                bat 'mvn clean package'
                bat 'move webapp\\target\\*.war webapp\\target\\myweb.war'
            }
        }
        stage("Deploy to Dev") {
            steps {
                sshagent(['tomcat-new']) {
                    bat """
                        pscp -scp -i path\\to\\privatekey.ppk -batch webapp\\target\\myweb.war centos@54.224.40.223:/opt/apache-tomcat-9.0.80/webapps

                        plink -i path\\to\\privatekey.ppk -batch centos@54.224.40.223 /opt/apache-tomcat-9.0.80/bin/shutdown.sh
                        
                        plink -i path\\to\\privatekey.ppk -batch centos@54.224.40.223 /opt/apache-tomcat-9.0.80/bin/startup.sh
                    """
                }
            }
        }
    }
}
