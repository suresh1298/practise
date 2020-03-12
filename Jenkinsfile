pipeline {
    agent any
    tools {
        maven 'maven'
    }
    stages {
        stage ("git scm") {
            steps {
                git 'https://github.com/suresh1298/Sample_Project'
            }
        }
        stage ("sonar") {
            environment {
                scannerHome = tool 'sonar'
            }
            steps {
                withSonarQubeEnv('sonar') {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }
        stage ("quality check") {
            steps {
                timeout(time: 10, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        stage ('maven build') {
            steps {
                sh 'mvn clean install'
            }
        }
    }
}
