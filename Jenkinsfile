pipeline {
    agent any
    stages {
        stage ("git scm") {
            steps {
                git 'https://github.com/suresh1298/Sample_Project'
            }
        }
        stage ("sonar") {
            enviroment {
                sonarHome = tool 'sonar'
            }
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }
    }
}
