pipeline {
  environment {
    registry = "montree/webserver"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Building Image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Push Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
	stage('Deployment') {
        steps {
			sh 'chmod +x modifyDeployment.sh'
			sh './modifyDeployment.sh $BUILD_NUMBER'
            sh 'kubectl apply -f 03-webserver-deployment-pattern-autoscale.yaml'
        }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry:$BUILD_NUMBER"
      }
    }
  }
}
