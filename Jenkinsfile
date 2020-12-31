pipeline {

  agent {
    kubernetes {
      label 'kubetest'  // all your pods will be named with this prefix, followed by a unique id
      idleMinutes 5  // how long the pod will live after no jobs have run on i
    }
  }

  stages {

    stage('Checkout Source') {
      steps {
        git url:'https://github.com/mbelbma/hellowhale.git', branch:'master'
      }
    }
    
      stage("Build image") {
            steps {
                script {
                    myapp = docker.build("mbel91/sourcing11:${env.BUILD_ID}")
                }
            }
        }
    
      stage("Push image") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com','dockerhub') {
                            myapp.push("latest")
                            myapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }

    
    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: 'nginx.yaml',kubeconfigId: 'kubeconfig')
        }
      }
    }

  }

}
