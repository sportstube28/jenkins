pipeline {
    agent any
    options {
		timeout(time: 60, unit: 'MINUTES') //Aborted the pipeline if it takes more than 5 hours
		buildDiscarder(logRotator(numToKeepStr: '30')) //Delete the builds older than 30
		disableConcurrentBuilds() // Disable concurrent builds
		timestamps() //Print the timestamps of each setp
	}
    stages {
        // Build stage
        stage('Build') {
            steps {
                //Delete node_modules and package-lock.json to avoid the build failure
                sh 'rm -rf node_modules/ package-lock.json'
                // Install required npm modules
                sh 'npm install'
                // Build the modules
                sh 'ng build'
            }
        }
        stage('Start') {
            steps {
                script{
                    withEnv(['JENKINS_NODE_COOKIE=dontKillMe']) {
                        // Run the application in background
                        sh "nohup ng serve &"
                    }
                }
            }
        }
    }
}

