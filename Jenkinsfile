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
                    if (env.branch_name == 'master') { 
                        withSonarQubeEnv('sonar') {
                            sh "${scannerHome}/bin/sonar-scanner"
                        }
                    } else { 
                        sh "echo this is not a master branch"
                    }
                }
            }
        }
        stage ("quality gate check") { 
            steps {
                script {
                    if (env.branch_name == 'master') {
                        timeout(time: 2, unit: 'MINUTES') {
                            waitForQualityGate abortPipeline: true
                        }
                    } else {
                        sh "echo this is not a master branch"
                    }
                }
            }
        }
    }
}
