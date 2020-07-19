pipeline {
    agent any 
       stages {
           stage ("git") {
               steps {
               checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/vrer2/Sample_Project.git']]]
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
