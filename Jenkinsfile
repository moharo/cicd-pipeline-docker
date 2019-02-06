pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('Build Docker Image') {
	  	steps {
	    script {
		app= docker.build("moharo/node-app")
		app.inside {
			sh 'echo $(curl localhost:8081)'
			}
		}
	    }
	}
	stage('Push Docker image') {
		steps {
		script {
		       docker.withRegistry('https://registry.hub.docker.com', 'MyDocker') {
			app.push("${env.BUILD_NUMBER}")
			app.push("latest")
			}
	             }
	        } 
	   }	
    }
}
