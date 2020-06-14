pipeline {
    agent any
    stages {
        stage ("SCM_CHECKOUT") {
            steps {
                node ("${NODE}") {
                    script {
                        if (env.BRANCH_NAME == 'develope') {
                            git credentialsId: '04756d9a-c324-4be1-be79-92c25aba8374', url: 'https://github.com/suresh1298/practise.git'
                        } else {
                            git branch: 'develope', credentialsId: '04756d9a-c324-4be1-be79-92c25aba8374', url: 'https://github.com/suresh1298/practise.git'
                        }
                    }
                }
            }
        }
        stage ("scan") {
            environment {
                scannerHome = tool 'sonar'
            }
            steps {
                node ("${node}") {
                    withSonarQubeEnv('sonarqube') {
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }
        stage ("quality") {
            steps {
                timeout(time: 10, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        stage ("maven") {
            parallel {
                stage ("buld in master") {
                    steps {
                        sh 'mvn clean install'
                    }
                }
                stage ("buld in slave") {
                    steps {
                        node ("${NODE}") {
                            sh 'mvn clean install'
                        }
                    }
                }
            }
        }
    }
}
