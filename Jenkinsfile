pipeline {
    agent any
    environment {
        registry = "sumaiyap/helloworld"
        registryCredential = 'docker-hub-credentials'
        DOCKERHUB_CREDENTIALS_PSW = 'docker'
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
                    sh "echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u sumaiyap --password-stdin"
                    sh "docker push $registry:$BUILD_NUMBER"
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
