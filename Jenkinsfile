pipeline {
    agent any
    stages {
        stage ("scm") {
            parallel {
                stage ("a") {
                    steps {
                        script {
                            if (env.BRANCH_NAME == 'master') {
                                sh "echo 'this is master baranch'"
                            } else {
                                sh "echo 'nothing to do'"
                            }
                        }
                    }
                }
                stage ("b") {
                    steps {
                        script {
                            if (env.SERVER_NAME == 'abc') {
                                sh "echo 'this is server abe'"
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
