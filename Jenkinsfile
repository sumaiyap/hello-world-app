pipeline {
    agent any
    environment {
        registry = "sumaiyap/helloworld"
        registryCredential = 'docker-hub-credentials'
        DOCKERHUB_CREDENTIALS = credentials('docker-cred')
    
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
                    sh "echo $DOCKERHUB_CREDENTIALS"
                    sh "docker login -u sumaiyap -p$DOCKERHUB_CREDENTIALS"
                    sh "docker push $registry:$BUILD_NUMBER"
                }
            }
        }

        stage('Cleaning up') {
            steps {
                sh "docker rmi $registry:$BUILD_NUMBER"
            }
        }

        stage('Deploy') {
            steps {
                script {
                    dir('kubernetes') {
                        def imageName = "$registry:$BUILD_NUMBER"
                    
                        sh "kubectl apply -f deployment.yaml --namespace=upgrad"  
                        sh "kubectl apply -f service.yaml --namespace=upgrad"                   
                        sh "kubectl set image deployment/hello-world-app hello-world-app=${imageName} --namespace=upgrad" 
                    } 
                }
            }
        }
    }
}
