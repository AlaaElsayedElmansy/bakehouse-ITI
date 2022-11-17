pipeline {
  agent { label "slave"}
  stages {
    stage('build') {
      steps {
        script {
       
            withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'username', passwordVariable: 'password')]) {
              sh """
        
                  docker login -u ${username} -p ${password}
                  docker build -t alaaelmansy/vfbakehouse .
                  docker push alaaelmansy/vfbakehouse
                 
              """
          }
        } 
      }
    }
    stage('deploy') {
    
      steps {
        script {

            withCredentials([file(credentialsId: 'service' ,variable: 'key')]) {
              sh """
                  gcloud auth activate-service-account k8s-terraform@alaa-368700.iam.gserviceaccount.com  --key-file ${key}
        
                """
            }
        


            withCredentials([file(credentialsId: 'kubeconfig', variable: 'KUBECONFIG')]) {
              sh """
                  
                  kubectl apply -f Deployment --kubeconfig=${KUBECONFIG}
                """
            }
        }
      }
    }
  }
}

