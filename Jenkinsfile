pipeline {
    agent any 
    options {
        timestamps()
        ansicolor("xterm")
    }
    stages {
        stage ("Checkout scm") {
            checkout scm
        }
    }
}