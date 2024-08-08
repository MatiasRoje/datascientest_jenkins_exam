pipeline {
  environment {
    imagename = 'matiasroje/root-service'
    registryCredential = 'dockerhub'
    dockerImage = ''
  }

  agent any

  stages {
    stage('Cloning Git') {
      steps {
        git([url: 'https://github.com/MatiasRoje/datascientest_jenkins_exam.git', branch: 'master', credentialsId: 'github'])
      }
    }

    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build(imagename, "root-service")
        }
      }
    }

    stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push("$BUILD_NUMBER")
            dockerImage.push('latest')
          }
        }
      }
    }

    // stage('Deploy to K8s') {
    //   steps{
    //     script {
    //       sh "sed -i 's,TEST_IMAGE_NAME,harshmanvar/node-web-app:$BUILD_NUMBER,' deployment.yaml"
    //       sh "cat deployment.yaml"
    //       sh "kubectl --kubeconfig=/home/ec2-user/config get pods"
    //       sh "kubectl --kubeconfig=/home/ec2-user/config apply -f deployment.yaml"
    //     }
    //   }
    // }
  }
}