pipeline {
    environment {
		server = Artifactory.newServer url: 'Artifactory'
		rtMaven = Artifactory.newMavenBuild()
	}
	
	agent any
	stages {
		stage('build') {
			steps {
				bat "mvn compile"
			}
		}
		stage('Artifactory configuration') {
			steps {
			}
		}
		stage('exec Maven') {
			steps {
				script {
				rtMaven.run pom: 'maven-example/pom.xml', goals: 'clean install', buildInfo: buildInfo
				}
			}
		}
		stage('Publish build info') {
			steps {
				script {
				server.publishBuildInfo buildInfo
				}
			}
		}
	}
}