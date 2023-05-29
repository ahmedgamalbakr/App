pipeline {
  agent {
    label 'dev'
  }
  
  stages {

    stage('Build and Push Docker Image') {
      steps {
        
        script {
          sh "docker build -t ahmedgamal22/simpleapp ."
        }
        
        script {
        withCredentials([usernamePassword(credentialsId: 'dockerhub',usernameVariable: 'DOCKER_USERNAME',passwordVariable: 'DOCKER_PASSWORD')]){

          sh "docker login -u ${DOCKER_USERNAME}  -p  ${DOCKER_PASSWORD} "
          sh "docker push ahmedgamal22/simpleapp"
        }
      }
    }
    }

    stage('CD -- Deploy to EKS') {
      steps {
            dir('./App_Deployment') {

        script {  

       sh """  
       
         kubectl delete deployment --all -n application
         kubectl apply -f app_namespace.yaml
         kubectl apply -f simpleApp_Deployment.yaml
         kubectl apply -f app_Service.yaml
        """

              }
          }
        }

        
      }
    }
  }


