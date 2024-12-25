pipeline {
    agent any
    tools {
        maven "MVN_HOME"
    }
    environment {
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "54.90.131.74:8081"
        NEXUS_REPOSITORY = "spring3"
        NEXUS_CREDENTIAL_ID = "nexus"
        PACKAGING = "war"  // Manually set the packaging type
    }
    stages {
        stage("Clone Code") {
            steps {
                script {
                    git 'https://github.com/Farsaan-tech/spring3-mvc-maven-xml-hello-world-1.git'
                }
            }
        }
        stage("Build with Maven") {
            steps {
                script {
                    sh 'mvn -Dmaven.test.failure.ignore=true install'
                }
            }
        }
        stage("Verify Build Output") {
            steps {
                script {
                    def artifactPath = "target/${PACKAGING}-artifact.${PACKAGING}"
                    echo "Expected artifact path: ${artifactPath}"
                    sh 'ls -al target/'
                }
            }
        }
        stage("Publish to Nexus") {
            steps {
                script {
                    def artifactPath = "target/ncodeit-hello-world-3.0.${PACKAGING}"
                    artifactExists = fileExists artifactPath
                    if (artifactExists) {
                        echo "*** File: ${artifactPath}, version ${BUILD_NUMBER}"
                        nexusArtifactUploader(
                            nexusVersion: env.NEXUS_VERSION,
                            protocol: env.NEXUS_PROTOCOL,
                            nexusUrl: env.NEXUS_URL,
                            groupId: 'com.ncodeit',
                            version: "${BUILD_NUMBER}",
                            repository: env.NEXUS_REPOSITORY,
                            credentialsId: env.NEXUS_CREDENTIAL_ID,
                            artifacts: [
                                [artifactId: 'ncodeit-hello-world', classifier: '', file: artifactPath, type: env.PACKAGING],
                                [artifactId: 'ncodeit-hello-world', classifier: '', file: "pom.xml", type: "pom"]
                            ]
                        )
                    } else {
                        error "*** File: ${artifactPath}, could not be found"
                    }
                }
            }
        }
    }
}
