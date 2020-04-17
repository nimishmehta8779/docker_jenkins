pipeline {
    agent any 
    environment{
        registry = "nimishmehta8779/myrepo"
        registryCredential = 'dockerhub'
        USERPASS = "password"
        USERNAME = "nimish"
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
                        sh 'echo $(curl localhost)'
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
                    app.push("latest")
                    }
                }
           }
        }
        stage ("Delete unwanted images") {
            when {
                branch 'master'
            }
            steps {
                sh "docker prune -a --force"
            }
        }
        stage ("Deploy in production") {
            when {
                branch 'master'
            }
            steps {
                input "Deploy to production ?"
                milestone(1)
                script {
                    sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@192.168.1.2 \"docker pull nimishmehta8779/ubuntu_nginx:latest"
                }
            }
        }
    }
}