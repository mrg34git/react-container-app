pipeline {
  agent {
    kubernetes {
     // label 'k8s-jnlp-agent-template'  // all your pods will be named with this prefix, followed by a unique id
      //inheritFrom 'k8s-jnlp-agent-template'
      idleMinutes 5  // how long the pod will live after no jobs have run on it
      yamlFile 'pod1.yaml'  // path to the pod definition relative to the root of our project 
      //defaultContainer 'maven'  // define a default container if more than a few stages use it, will default to jnlp container
     // defaultContainer 'mrg3'  
    }
 
  }
  environment {
    BUILD_TAG = "${env.BUILD_TAG}"
    PODNAME = "${env.NODE_NAME}"
    NAME_SPACE = "jenkins"
    //${env.JOB_NAME}
    
  }
  stages {
    stage('SCM checkout') {
        steps {
           echo 'Clone repo...'
           //sh 'git pull origin'
           checkout scm           
        }
    }
    stage('Deployment') {
      steps {
        container('k8s-agent-alpine') {
          //echo "Database engine is ${DB_ENGINE}"
         echo "BUILD NAME: ${BUILD_TAG}"
         echo "NODE NAME: ${env.NODE_NAME}"
        //container('k8s-jnlp-agentp') {
          echo 'Deploying....'
        //sh ' kubectl delete -f react-test.yaml'
          sh 'hostname'
          sh 'pwd'
          sh 'ls -l /usr/local/k8s/kubectl'
          sh 'whoami'
          sh 'env'
          withKubeConfig(caCertificate: '', clusterName: 'kubernetes', contextName: '', credentialsId: 'k8s_file_id') {
          //withKubeConfig(caCertificate: '', clusterName: 'kubernetes', contextName: '', credentialsId: 'k8s_file_id', serverUrl: 'https://apphub01:6443') {
              echo "Cleaning up any kubectl agent in completed status"
              sh 'kubectl delete pod --field-selector=status.phase==Succeeded -n jenkins'
              sh 'kubectl get pods'
              sh 'kubectl get nodes'
              sh '/usr/local/k8s/kubectl get nodes,pods,services,deployment -o wide --all-namespaces '
              sh 'kubectl delete -f ./jenkins_build_deploy.yaml && kubectl create -f ./jenkins_build_deploy.yaml'
              //sh 'kubectl create -f ./jenkins_build_deploy.yaml'
              sh 'kubectl get pods'
            //  sh 'kubectl delete pod "${PODNAME}" -n jenkins'
           }  
          //sh 'kubectl get nodes'
         // sh 'kubectl delete -f .jenkins_build_deploy.yaml' 

          ///sh ' kubectl apply -f jenkins_build_deployment.yaml'
        }
     }
    }
//    stage('Build') {
//      steps {  // no container directive is needed as the maven container is the default
//        sh "mvn clean install"   
//      }
 //   }
 //   stage('Build React Docker Image') {
 //     steps {
 //       container('react-container-app') {
 //           sh 'hostname'
 //           sh 'nginx -v'
 //           sh 'uname -a'
 //           sh 'echo "current directory is" &&  pwd'
 //           sh 'ls -l'


          //sh "docker build -t mrg34dck/react-container-app:v.latest ."  // when we run docker in this step, we're running it via a shell on the docker build-pod container, 
          //sh "docker push mrg34dck/react-container-app:v.latest"        // which is just connecting to the host docker deaemon
   //     }  
   //   }
    //}
    
  }
  post {
    always {
       echo 'Portal image is deployed in to Kubernetes '
    }
    success {
      echo 'Successfully!'
    }
    failure {
      echo 'Failed!'
    }
    unstable {
      echo 'This will run only if the run was marked as unstable'
    }
    changed {
       echo 'This will run only if the state of the Pipeline has changed'
       echo 'For example, if the Pipeline was previously failing but is now successful'
    }
  }
}