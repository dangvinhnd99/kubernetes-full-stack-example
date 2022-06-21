pipeline {
  agent any
  environment {
    BUILD_SCRIPTS_GIT="https://github.com/dangvinhnd99/kubernetes-full-stack-example.git"
    BUILD_SCRIPTS='mypipeline'
    BUILD_HOME='/var/lib/jenkins/workspace'
  }
  stages {
    stage('Checkout: Code') {
          steps {
            sh "
                git config --global user.email 'dangvinhnd99@gmail.com';\
                git config --global user.name 'dangvinhnd99';\
                git 'https://github.com/dangvinhnd99/kubernetes-full-stack-example.git' "
           
        }
				}
	stage('Docker build'){
			steps {
				echo 'building your app!'
				sh 'docker version'
				dir ("spring-boot-student-app-api"){
				sh 'mvn package'
								 }
			}
				}
	stage('Mvn build'){
			steps {
				dir ("react-student-management-web-app"){
				sh 'docker build -t vinhbk99nd/student-app-client .'
				}
			}
	}
		
	stage('Push') {
			steps {
				echo 'testing your app!'
				sh 'docker  push vinhbk99nd/student-app-client'
				sh 'docker push registry.hub.docker.com/source/vinhbk99nd/student-app-api'
			}
	}
   }
  
}

