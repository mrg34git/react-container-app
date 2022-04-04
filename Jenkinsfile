pipeline {
  agent {
    kubernetes {
      label 'jenkins-react'  // all your pods will be named with this prefix, followed by a unique id
      idleMinutes 5  // how long the pod will live after no jobs have run on it
      yamlFile 'pod.yaml'  // path to the pod definition relative to the root of our project 
      //defaultContainer 'maven'  // define a default container if more than a few stages use it, will default to jnlp container
    }
 
  }
  //environment {
  //  CHESS_PC1 = env.CHESS_PC1  
  //}
  stages {
    stage('SCM checkout') {
        steps {
           echo 'Clone repo...'
           //sh 'git pull origin'
           checkout scm           
        }
    }
    //stage('Deployment') {
    //  steps {
    //    container('k8s-alpine-agent') {
    //      echo 'Deploying....'
        //sh ' kubectl delete -f react-test.yaml'
    //      sh 'hostname'
    //      sh 'pwd'
          //sh 'ls -l /usr/local/k8s/kubectl'
    //      sh 'whoami'
    //      sh 'env'
        //sh ' kubectl apply -f jenkins_build_deployment.yaml'
    //    }
    //  }
    //}
//    stage('Build') {
//      steps {  // no container directive is needed as the maven container is the default
//        sh "mvn clean install"   
//      }
 //   }
    stage('Build React Docker Image') {
      steps {
        container('react-container-app') {
            sh 'hostname'
            sh 'nginx -v'
            sh 'uname -a'
            sh 'echo "current directory is" &&  pwd'
            sh 'ls -l'


          //sh "docker build -t mrg34dck/react-container-app:v.latest ."  // when we run docker in this step, we're running it via a shell on the docker build-pod container, 
          //sh "docker push mrg34dck/react-container-app:v.latest"        // which is just connecting to the host docker deaemon
        }  
      }
    }
    
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