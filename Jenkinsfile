#!/usr/bin/env groovy

properties([parameters([
		booleanParam(defaultValue: false, description: "Publish library", name: "PUBLISH_JOB")
])])

node(label: "jdk11") {

	stage("Prepare") {
		checkout scm
	}

	withCredentials([usernamePassword(credentialsId: "nexusCredentials", usernameVariable: "ORG_GRADLE_PROJECT_nexusTroiiNetUser", passwordVariable: "ORG_GRADLE_PROJECT_nexusTroiiNetPassword")]) {

		final gradleOptions = env.GRADLE_OPTS ? [GRADLE_OPTS: env.GRADLE_OPTS] : [:]
		gradleOptions["ORG_GRADLE_PROJECT_nexusTroiiNetUser"] = env.ORG_GRADLE_PROJECT_nexusTroiiNetUser
		gradleOptions["ORG_GRADLE_PROJECT_nexusTroiiNetPassword"] = env.ORG_GRADLE_PROJECT_nexusTroiiNetPassword

		stage("Build") {
			sh "./gradlew -Dorg.gradle.internal.launcher.welcomeMessageEnabled=false build"
		}

		stage("Publish") {
			if (currentBuild.result == null) {
				if (params.PUBLISH_JOB) {
					sh "./gradlew publish"
					slack("good", "Publishing libraries for ${currentBuild.fullDisplayName} finished")
				}
			}
		}

	}
}
