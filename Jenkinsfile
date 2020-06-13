pipeline {
    agent any
    stages {
        stage ("scm") {
            parallel {
                stage ("master") {
                    steps {
                        node ("jenkins_slave") {
                            script {
                                if (env.BRANCH_NAME == 'master') {
                                    git credentialsId: '7778fd25-578d-48df-b454-17fe5ca8baa0', url: 'https://github.com/suresh1298/practise'
                                } else {
                                    sh "echo 'ur done in develope master'"
                                }
                            }
                        }
                    }
                }
                stage ("develop") {
                    steps {
                        node ("jenkins_slave") {
                            script {
                                if (env.SERVER_NAME == 'develop') {
                                    git branch: 'develope', credentialsId: '7778fd25-578d-48df-b454-17fe5ca8baa0', url: 'https://github.com/suresh1298/practise.git'
                                } else {
                                    sh "echo 'its in develope develope'"
                                }
                            }
                        }
                    }
                }
            }
        }
    }
}
