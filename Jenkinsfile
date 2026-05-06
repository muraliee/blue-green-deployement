
pipeline {
  agent any
  
   
  stages {
    stage("Clone code from GitHub") {
            steps {
                script {
                    checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'GITHUB_CREDENTIALS', url: 'https://github.com/muraliee/Deploy-NodeApp-to-AWS-EKS-using-Jenkins-Pipeline']])
                }
            }
        }
     
   
    stage('Blue Deployement') {
            steps {
              script {
                sh ('aws eks update-kubeconfig --name mohan-cluster2 --region us-east-2')
                sh "kubectl get ns"
                sh "kubectl apply -f blue.yaml"
        }
      }
    }
         
    stage('Green Deployement') {
        steps {
               script {
                 sh ('aws eks update-kubeconfig --name mohan-cluster2 --region us-east-2')
                 sh "kubectl get ns"
                 sh "kubectl apply -f green.yaml"
        }
      }
    }
    stage('switch Deployement Blue and Gree') {
        steps {
               script {
                 sh ('aws eks update-kubeconfig --name mohan-cluster2 --region us-east-2')
                 
                 sh "kubectl apply -f service.yaml"
        }
      }
    }

  }
}
