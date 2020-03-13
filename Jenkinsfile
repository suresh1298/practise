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
        }pipeline {
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
                scannerHome = tool 'sonarqube'
            }
            steps {
                withSonarQubeEnv('sonarqube') {
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
    post {
        success {
            emailext (
                to: 'reddysuresh1298@gmail.com',
                subject: "JOB: ${env.JOB_NAME} - SUCCESS",
                body: "JOB SUCCESS - \"${env.JOB_NAME}\" Build No: ${env.BUILD_NUMBER}\n\nClick on the below link to view the logs:\n ${env.BUILD_URL}\n"
            )
        }
        failure {
            emailext (
                to: 'reddysuresh1298@gmail.com',
                subject: "JOB: ${env.JOB_NAME} - FAILURE",
                body: "JOB FAILURE - \"${env.JOB_NAME}\" Build No: ${env.BUILD_NUMBER}\n\nClick on the below link to view the logs:\n ${env.BUILD_URL}\n"
            )
        }
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
    post {
        success {
            emailext (
                to: 'reddysuresh1298@gmail.com',
                subject: "JOB: ${env.JOB_NAME} - SUCCESS",
                body: "JOB SUCCESS - \"${env.JOB_NAME}\" Build No: ${env.BUILD_NUMBER}\n\nClick on the below link to view the logs:\n ${env.BUILD_URL}\n"
            )
        }
        failure {
            emailext (
                to: 'reddysuresh1298@gmail.com',
                subject: "JOB: ${env.JOB_NAME} - FAILURE",
                body: "JOB FAILURE - \"${env.JOB_NAME}\" Build No: ${env.BUILD_NUMBER}\n\nClick on the below link to view the logs:\n ${env.BUILD_URL}\n"
            )
        }
    }
}
