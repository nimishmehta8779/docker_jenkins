pipeline {
    agent any 
    environment{
        registry = "nimishmehta8779/myrepo"
        registryCredential = 'dockerhub'
    }
    stages {
        stage ('Build') {
            when {
                branch 'master'
            }
            steps {
                script {
                    app = docker.build("nimishmehta8779/ubuntu_nginx")
                    app.inside{
                        sh 'echo $(localhost:8080)'
                    }
                }
            }
        }
        stage ("Pushing Image") {
            when {
                branch 'master'
            }
            steps {
                script {
                    docker.withRegistry( 'https://registry.hub.docker.com', registryCredential) {
                        dockerImage.push()
                    }
                }
           }
        }
        stage ("Remove unused Docker image") {
            steps {
                sh "docker rmi nimishmehta8779/ubuntu_nginx"
            }
        }
    }
}