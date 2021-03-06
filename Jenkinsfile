pipeline {
    agent any 
    environment{
        registry = "nimishmehta8779/myrepo"
        registryCredential = 'dockerhub'
        USERPASS = "password"
        USERNAME = "nimish"
        PROD_IP = "192.168.1.2"
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
                sh "docker image prune -a --force"
            }
        }
        stage ("Deploy to production") {
            when {
                branch 'master'
            }
            steps {
//                input "Deploy to production ?"
//                milestone (1)
                script {
                    sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@$PROD_IP \"docker pull nimishmehta8779/ubuntu_nginx:latest\""
                    try {
                        sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@$PROD_IP \"docker stop ubuntu_nginx\""
                        sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@$PROD_IP \"docker rm ubuntu_nginx\""
                    } catch(err)
                    {
                        echo: 'caught error: $err'
                    }
                    sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@$PROD_IP \"docker run --restart always --name ubuntu_nginx -p 80:80  -d nimishmehta8779/ubuntu_nginx:latest\""
                }
            }
        }
    }
}