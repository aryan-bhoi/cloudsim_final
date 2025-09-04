pipeline {
    agent none  // define agents per stage

    stages {
        stage('Checkout') {
            agent { label 'slave1' }
            steps {
                echo "Checking out code on ${env.NODE_NAME}"
                git 'https://github.com/aryan-bhoi/cloudsim_final.git'
            }
        }

        stage('Compile') {
            agent { label 'slave1' }
            steps {
                echo "Compiling on ${env.NODE_NAME}"
                sh 'mvn clean compile'
            }
        }

        stage('Unit Tests') {
            agent { label 'slave2' }
            steps {
                echo "Running tests on ${env.NODE_NAME}"
                sh 'mvn test'
            }
        }

        stage('Package') {
            agent { label 'slave1' }
            steps {
                echo "Packaging on ${env.NODE_NAME}"
                sh 'mvn package'
            }
        }
    }

    post {
        always {
            echo "Cleaning workspace on ${env.NODE_NAME}"
            cleanWs()
        }
        success {
            echo "Pipeline completed successfully!"
        }
        failure {
            echo "Pipeline failed. Check logs!"
        }
    }
}