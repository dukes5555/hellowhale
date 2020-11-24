pipeline {

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git url:'https://github.com/dukes5555/eks-workshop-sample-api-service-go.git', branch:'master'
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
      withKubeConfig([credentialsId: 'jenkins_kconfig', serverUrl: 'https://A1216214A6E9FECA7124B1BAC5472DD6.yl4.us-east-1.eks.amazonaws.com']) {
        sh 'kubectl apply -f hello-k8s.yml -n devteam3'
      }
      }

  }

}
