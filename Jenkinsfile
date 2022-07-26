pipeline {
  agent any
  environment {
    BUILD_SCRIPTS_GIT="https://github.com/dangvinhnd99/kubernetes-full-stack-example.git"
    BUILD_SCRIPTS='mypipeline'
    BUILD_HOME='/var/lib/jenkins/workspace'
  }
  stages { 
    stage('gitclone') {
          steps {
                git  'https://github.com/dangvinhnd99/kubernetes-full-stack-example.git' 
        }
				}
	stage('Mvn build'){
			steps {
				echo 'building your app'
				dir ("spring-boot-student-app-api"){
				sh " mvn install"
								 }
			}
				}
	stage('Docker build'){
			steps {
				dir ("react-student-management-web-app"){
				sh "docker build -t vinhbk99nd/student-app-client:0.0.4 ."
				}
			}
	}
		
	stage('Login') {
			steps {
				script{
					withCredentials([string(credentialsId: 'vinhpass', variable: 'dockerpass')]) {
						sh 'docker login -u vinhbk99nd -p ${dockerpass}'}
				}
				
			}
	}
	  stage('Push') {
			steps {
				echo 'testing your app!'
				sh "docker push vinhbk99nd/student-app-client:0.0.3"
				// sh "docker tag 5a8db398eaa7 vinhbk99nd/student-app-api:0.0.1-SNAPSHOT"
				sh "docker push vinhbk99nd/student-app-api:0.0.1-SNAPSHOT"
			}
	}
	   stage("Add repo"){
		  	steps{
        			sh 'helm repo add prometheus-community https://prometheus-community.github.io/helm-charts'
        			sh 'helm repo add bitnami https://charts.bitnami.com/bitnami'
        			sh 'helm repo add istio https://istio-release.storage.googleapis.com/charts'
        			sh 'helm repo update'
		   }
    }
    
   	   stage("Deployment istio"){
		   	steps{
       				// sh 'kubectl create namespace istio-system'
        			sh 'helm upgrade istio-base istio/base -n istio-system --install'
        			sh 'helm upgrade istiod istio/istiod -n istio-system --wait --install'
        			// sh 'kubectl create namespace istio-ingress'
        			sh 'kubectl label namespace default istio-injection=enabled --overwrite'
			}
    }
    
    	   stage("Deployment react"){
		   	steps{
//         sh 'minikube start --driver=none --kubernetes-version v1.23.8'
      		 		sh 'helm upgrade vinhhelm helm_chart/ --install'
       		 		dir("helm_chart"){
          	 		sh 'kubectl apply -f istio_ingress.yaml'
		 }
       		 		//sh 'helm upgrade istio-ingress istio/gateway -f ip-external.yaml --install'
			}
    }
    
   	 stage("Deployment prometheus"){
		 	steps{
       				sh 'helm upgrade prometheus prometheus-community/prometheus --install'
        			// sh 'kubectl expose service prometheus-server --type=NodePort --target-port=9090 --name=prometheus-server-np'
//         sh 'minikube service prometheus-server-np'
			}
    }     
    	 stage("Deployment graffana"){
		 steps{
        		sh 'helm upgrade grafana bitnami/grafana --install'
        		//sh 'kubectl expose service grafana --type=NodePort --target-port=3000 --name=grafana-np'
		 }
//         sh 'echo "Password: $(kubectl get secret grafana-admin --namespace default -o jsonpath="{.data.GF_SECURITY_ADMIN_PASSWORD}" | base64 -d)"'
//         sh 'minikube service grafana-np'
    }
   }
  
}

