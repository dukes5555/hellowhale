pipeline {

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git url:'https://github.com/dukes5555/hellowhale.git', branch:'master'
      }
    }

      stage("Build image") {
            steps {
                script {
                    myapp = docker.build("dukes5555/test-app:${env.BUILD_ID}")
                }
            }
        }

      stage("Push image") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                            myapp.push("latest")
                            myapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }


      stage('Deploy k8s') {
        steps {
          withKubeConfig([credentialsId: '', serverUrl: 'https://0DE2BFAB8F2B9713C6D4A228829C7108.gr7.us-east-1.eks.amazonaws.com']) {
          sh 'kubectl apply -f hellowhale.yml -n devteam1'
          }
        }
      }
  }

}
