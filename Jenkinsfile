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
                    sh "docker login -u sumaiyap -p$docker"
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
