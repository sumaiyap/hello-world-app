pipeline {
    agent any
    environment {
        registry = "sumaiyap/helloworld"
        registryCredential = 'docker-hub-credentials'
    }

    stages {
        stage('Build') {
            steps {
                script {
                    dir('hello-world-app') {

                        dockerImage = docker.build registry + ":$BUILD_NUMBER"
                    }
                }
            }
        }

        stage('Push') {
            steps {
                script {
                    docker.withRegistry('', registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }

        stage('Cleaning up') {
            steps {
                sh "docker rmi $registry:$BUILD_NUMBER"
            }
        }
    }
}
