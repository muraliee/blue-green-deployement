pipeline {
  agent any
   parameters{
     choice(name:'ENV',choices:['blue','green'],description:'Select deploye environment')
	 }
       
   
  stages {
    stage("Clone code from GitHub") {
            steps {
                script {
                    checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'GITHUB_CREDENTIALS', url: 'https://github.com/muraliee/blue-green-deployement']])
                }
            }
        }
     
   
    stage('Blue Deployement') {
	
	when {
                expression { params.ENV == 'blue' }
            }
	
	
	
            steps {
              script {
                sh ('aws eks update-kubeconfig --name mohan-cluster3 --region us-east-2')
                sh "kubectl get ns"
                sh "kubectl apply -f blue.yaml"
                     
                    def status = sh(
                        script: 'kubectl rollout status deployment/myapp --timeout=30s',
                        returnStatus: true
                    )

                    if (status == 0) {
                        sh "kubectl apply -f service-blue.yaml"
                    } else {
                        echo "application stable in green environment"
                    }
                
				
                
        }
      }
    }
	
	stage('Gree Deployement') {
	
	when {
                expression { params.ENV == 'green' }
            }
	
	
	
            steps {
              script {
                sh ('aws eks update-kubeconfig --name mohan-cluster3 --region us-east-2')
                sh "kubectl get ns"
                sh "kubectl apply -f green.yaml"
                     
                    def status = sh(
                        script: 'kubectl rollout status deployment/myapp --timeout=30s',
                        returnStatus: true
                    )

                    if (status == 0) {
                        sh "kubectl apply -f service-green.yaml""
                    } else {
                        echo "application stable in blue environment"
                    }
                 }
               }
    }
         
    
    
}
}