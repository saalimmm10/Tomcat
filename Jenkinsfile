pipeline {
    agent any

    tools {
        jdk 'JDK21'
        maven 'Maven-3.9.16'
    }

    stages {

        stage('Build') {
            steps {
                bat 'java --version'
                bat 'mvn --version'
                bat 'mvn clean package'
            }
        }

        stage('Deploy') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'tomcat-credentials',
                        usernameVariable: 'TOMCAT_USER',
                        passwordVariable: 'TOMCAT_PASS'
                    )
                ]) {
                    bat '''
                        curl --fail -u "%TOMCAT_USER%:%TOMCAT_PASS%" ^
                        -T "target\\jenkins-tomcat-demo-1.0.war" ^
                        "http://localhost:7080/manager/text/deploy?path=/jenkins-tomcat-demo&update=true"
                    '''
                }
            }
        }
    }
}