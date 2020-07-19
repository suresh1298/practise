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
                dir("/opt/docker/") {
                    sh "docker build -t tomcat:1.0 ."
                }
            }
        }
    }
}
