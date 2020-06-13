pipeline {
    agent any
    stages {
        stage ("SCM_CHECKOUT") {
            steps {
                node ("${NODE}") {
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
        stage ("scan") {
            environment {
                scannerHome = tool 'sonar'
            }
            steps {
                node ("${node}") {
                    withSonarQubeEnv('sonarQube') {
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
        stage ("nexus") {
            parallel {
                stage ("deploy in master") {
                    steps {
                        node ("${NODE}") {
                            script {
                                // Read POM xml file using 'readMavenPom' step , this step 'readMavenPom' is included in: https://plugins.jenkins.io/pipeline-utility-steps
                                pom = readMavenPom file: "pom.xml";
                                // Find built artifact under target folder
                                filesByGlob = findFiles(glob: "target/*.${pom.packaging}");
                                // Print some info from the artifact found
                                echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
                                // Extract the path from the File found
                                artifactPath = filesByGlob[0].path;
                                // Assign to a boolean response verifying If the artifact name exists
                                artifactExists = fileExists artifactPath;
                                if(artifactExists) {
                                    echo "*** File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}";
                                    nexusArtifactUploader(
                                        nexusVersion: NEXUS_VERSION,
                                        protocol: NEXUS_PROTOCOL,
                                        nexusUrl: NEXUS_URL,
                                        groupId: pom.groupId,
                                        version: pom.version,
                                        repository: NEXUS_REPOSITORY,
                                        credentialsId: NEXUS_CREDENTIAL_ID,
                                        artifacts: [
                                            // Artifact generated such as .jar, .ear and .war files.
                                            [artifactId: pom.artifactId,
                                            classifier: '',
                                            file: artifactPath,
                                            type: pom.packaging],
                                            // Lets upload the pom.xml file for additional information for Transitive dependencies
                                            [artifactId: pom.artifactId,
                                            classifier: '',
                                            file: "pom.xml",
                                            type: "pom"]
                                            ]
                                            );
                                    
                                } else {
                                    error "*** File: ${artifactPath}, could not be found";
                                }
                            }
                        }
                    }
                }
            }
        }
        stage ("depeloy") {
            parallel {
                stage ("deploy in jenkins") {
                    steps {
                        sh "cp target/*.war /opt/tomcat/webapps"
                    }
                }
            }
        }
        stage ("deploy in slave") {
            steps {
                node ("${NODE}") {
                    sh "sudo cp target/*.war /opt/tomcat/webapps"
                }
            }
        }
    }
}
