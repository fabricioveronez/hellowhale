pipeline {
  agent any

  stages {
    stage('Checkout Source') {
      steps {
        git url:'https://github.com/fabricioveronez/hellowhale.git', branch:'master'
      }
    }

      stage('Build image') {
      steps {
        script {
          myapp = docker.build("fabricioveronez/hellowhale:${env.BUILD_ID}")
        }
      }
      }

      stage('Push image') {
      steps {
        script {
          docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                            myapp.push('latest')
                            myapp.push("${env.BUILD_ID}")
          }
        }
      }
      }

    stage('Deploy App') {
      agent {
        kubernetes {
          cloud 'kubernetes'
        }
      }
      steps {
        script {
          kubernetesDeploy(configs: 'hellowhale.yml', kubeconfigId: 'kubeconfig')
        }
      }
    }
  }
}
