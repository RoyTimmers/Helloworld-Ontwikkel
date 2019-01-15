node {
    def server = Artifactory.newServer url: 'Artifactory'
    def rtMaven = Artifactory.newMavenBuild()
    def buildInfo

    stage ('Artifactory configuration') {
        rtMaven.tool = 'Local Maven' // Tool name from Jenkins configuration
        rtMaven.deployer releaseRepo: 'libs-release-local', snapshotRepo: 'libs-snapshot-local', server: server
        rtMaven.resolver releaseRepo: 'libs-release', snapshotRepo: 'libs-snapshot', server: server
        buildInfo = Artifactory.newBuildInfo()
    }

    stage ('Exec Maven') {
        rtMaven.run pom: 'java-maven-junit-helloworld-master/pom.xml', goals: 'test', buildInfo: buildInfo
    }

    stage ('Publish build info') {
        server.publishBuildInfo buildInfo
    }
}