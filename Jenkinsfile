pipeline {
    environment {
        imagename = "yarlov21/jenkinspractice"
        registryCredential = "jenkins_docker"
        dockerimage = ''
    }
    agent any
    stages {
        stage('Cloning Git') {
            steps {
                git([url: 'https://github.com/yared-shewarade/jenkinspractice.git', branch:'main', credentialsId: 'jenkins_ssh_key'])
            }
        }
        stage('Building image') {
            steps{
                script {
                    dockerImage = docker.build imagename
                }
            }
        }
        stage('Deploy Image'){
            steps{
                script {
                    docker.withRegistry('', registryCredential ) {
                        dockerImage.push("$BUILD_NUMBER")
                        dockerImage.push('latest')
                    }
                }
            }
        }
        stage('Remove Unused docker image') {
            steps{
                sh "docker rmi $imagename:$BUILD_NUMBER"
                sh "docker rmi $imagename:latest"
            }
        }
    }
}
