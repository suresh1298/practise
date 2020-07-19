pipeline {
    agent any 
    stages {
        stage ("git") {
            steps {
                git 'https://github.com/suresh1298/practise.git'
            }
        }
        stage ("sonar analasys") {
            environment {
                scannerHome = tool 'scanner'
            }
            steps {
                script {
                    withSonarQubeEnv('sonar') {
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }
        stage ("quality gate check") { 
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        stage ("mvn") {
            steps {
                sh "sudo cp /opt/docker/java/java.sh ."
                sh "sudo cp /opt/docker//maven/maven.sh ."
                sh "sudo cp /opt/docker/tomcat/manager.xml ."
                sh "sudo cp /opt/docker/tomcat/tomcat-users.xml ."
            }
        }
        stage ("imagee build") {
            steps {
                script {
                    docker.build ("tomcat:latest")
                }
            }
        }
    }
}
