pipeline {
	def server = Artifactory.newServer url: Artifactory
	def rtMaven = Artifactory.newMavenBuild()
	
	agent any
	stages {
		stage('build') {
			steps {
				bat "mvn compile"
			}
		}
		stage('Artifactory configuration') {
			steps {
				rtMaven.tool = 'Local Maven' // Tool name from Jenkins configuration
				rtMaven.deployer releaseRepo:'libs-release-local', 'snapshotRepo:'libs-snapshot-local', server: server
				rtMaven.resolver releaseRepo:'libs-release', 'snapshotRepo'libs-snapshot', server: server
				def buildInfo = Artifactory.newBuildInfo()
				buildInfo.env.capture = true
			}
		}
		stage('exec Maven') {
			steps {
				rtMaven.run pom: 'maven-example/pom.xml', goals: 'clean install', buildInfo: buildInfo
			}
		}
		stage('Publish build info') {
			steps {
				server.publishBuildInfo buildInfo
			}
		}
	}
}