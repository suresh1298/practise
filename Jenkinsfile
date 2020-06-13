pipeline {
    agent any
    stages {
        stage ("scm") {
            parallel {
                stage ("my_repo") {
                    steps {
                        node (${NODE}) {
                            script {
                                if (env.BRANCH_NAME == 'master') {
                                    git credentialsId: '7778fd25-578d-48df-b454-17fe5ca8baa0', url: 'https://github.com/suresh1298/practise'
                                } else {
                                    git branch: 'develope', credentialsId: '7778fd25-578d-48df-b454-17fe5ca8baa0', url: 'https://github.com/suresh1298/practise.git'
                                }
                            }
                        }
                    }
                }
                stage ("rajesh") {
                    steps {
                        node ("jenkins_slave") {
                            script {
                                if (env.SERVER_NAME == 'rajesh') {
                                    git branch: 'develope', credentialsId: '7778fd25-578d-48df-b454-17fe5ca8baa0', url: 'https://github.com/suresh1298/practise.git'
                                } else {
                                    sh "echo 'nothing to do'"
                                }
                            }
                        }
                    }
                }
            }
        }
    }
}
