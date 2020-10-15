pipeline {
    agent any

    environment {
        dockerImage = ''
    }

    stages {
        stage('Build image') {
            steps {
                script {
                    dockerImage = docker.build("montree/webserver")
                }
            }
        }
        stage('Push image') {
            steps {
                script {
                    withDockerRegistry(
                        credentialsId: 'dockerhub',
                        url: 'https://index.docker.io/v1/') {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Deployment') {
            steps {
                sh 'kubectl apply -f 03-webserver-deployment-pattern-autoscale.yaml';
            }
        }
    }
}
 