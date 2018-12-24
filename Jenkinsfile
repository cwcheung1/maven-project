pipeline {
    agent any

    tools {
        maven 'localMaven'
    }

    stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }

            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

	stage('Deploy to Staging'){
	    steps {
		build job: 'deploy-to-staging'
	    }
	}

	stage ('Deploy to Production') {
	    steps {
		timeout(time:5, units:'DAYS'){
		    input message: "Approve deploy to production?"
		}

		build job: "deploy-to-prod"
	    }
	    post {
		success {
		    echo 'Code deployed to Production'
		}

		failure {
		    echo 'Code deployment error - not deployed to Production'
		}

	    }
	}
    }
}
