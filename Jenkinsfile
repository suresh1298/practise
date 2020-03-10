pipeline {
    agent any
    stages {
        stage ("scm") {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/suresh1298/Sample_Project']]])
            }
        }
        stage ("touch") {
            steps {
                sh "touch suresh"
            }
        }
        stage ("export"){
            steps {
                sh "export"
            }
        }
    }
}
