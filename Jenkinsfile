pipeline {
    agent any 
    stages {
        stage ("git") {
            steps {
                git 'https://github.com/suresh1298/practise.git'
            }
        }
        stage ("sonar analysis") {
            steps {
                environment {
                    scannerHome = tool 'sonarscanner'
                }
           }
        }
        stage ("quality gate") {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
           }
        }
        stage ("mvn") {
            steps {
                sh "mvn package"
            }
        }
    }
}
