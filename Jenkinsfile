node {
    def app
    stage ('Check out scm') {
        checkout scm
    }

    stage ('Build image') {
        app = docker.build("nimshmehta8779/ubuntu_nginx"), -f './Dockerfile .'
    }

    stage ('Test image') {
        app.inside {
            sh 'echo "Test Passed"'
        }
    }
}

