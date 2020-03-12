pipeline {
    agent any
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
    }
}
