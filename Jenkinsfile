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
                            } if (env.BRANCH_NAME == 'develope') {
                                sh "echo 'this is develope branch'"
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
                            } if (env.SERVER_NAME == 'xyz') {
                                sh "echo 'this is server xyz'"
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
                                
