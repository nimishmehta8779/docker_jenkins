pipeline{
    agent any
    stages{
        stage('Checkout'){
            steps{
                echo "========Checking out scm ========"
                checkout scm
            }
        }
        stage('Build Docker Image'){
            when {
                branch 'master'
            }
            steps{
                script {
                    echo "===========Building Docker Image========="
                    app = docker.build("nimishmehta8779/nginx")
                }
            }
        }
        stage('Test image')
        {
            app.inside{
                sh 'echo "Test Passed"'
            }
        }
    }
}
