pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
            }
        }
    }
}
stage ('Artifactory configuration') {
            steps {
                rtServer (
                    id: "JFrog",
                    url: http://54.198.234.177:8081/artifactory,
                    credentialsId: JFROG
                )

                rtMavenDeployer (
                    id: "MAVEN_DEPLOYER",
                    serverId: "http://54.198.234.177:8081/artifactory",
                    releaseRepo: "libs-release-local",
                    snapshotRepo: "libs-snapshot-local"
                )

                rtMavenResolver (
                    id: "MAVEN_RESOLVER",
                    serverId: "http://54.198.234.177:8081/artifactory",
                    releaseRepo: "libs-release",
                    snapshotRepo: "libs-snapshot"
                )
            }
        }

        stage ('Exec Maven') {
            steps {
                rtMavenRun (
                    tool: MAVEN_TOOL, // Tool name from Jenkins configuration
                    pom: 'maven-example/pom.xml',
                    goals: 'clean install',
                    deployerId: "MAVEN_DEPLOYER",
                    resolverId: "MAVEN_RESOLVER"
                )
            }
        }

        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "http://54.198.234.177:8081/artifactory"
                )
            }
        }
    }
}
