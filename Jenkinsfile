node {
    def app
    stage ('Check out scm') {
        checkout scm
    }

    stage ('Build image') {
        app = docker.build("nimshmehta8779/nginx")
    }

    stage ('Test image') {
        app.inside {
            sh 'echo "Test Passed"'
        }
    }
}

