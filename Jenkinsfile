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
            sh "mkdir -p $WORKSPACE/repo;\
                git config --global user.email 'email@address.com';\
                git config --global user.name 'myname';\
                git config --global push.default simple;\
                git clone $BUILD_SCRIPTS_GIT repo/$BUILD_SCRIPTS"
            sh "chmod -R +x $WORKSPACE/repo/$BUILD_SCRIPTS"
          }
	stage('Docker build'){
			steps {
				echo 'building your app!'
				sh 'docker version'
				sh 'docker build -t vinhbk99nd/student-app-api'
				  
				}
			}
	stage('Push') {
			steps {
				echo 'testing your app!'
				sh 'docker  push vinhbk99nd/student-app-api'
			}
	}
   }
 
   }
	
  post {
    always {
       cleanWs()
    }
  }
}
