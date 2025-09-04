pipeline {
    agent none  // We'll define agents per stage

    environment {
        MAVEN_HOME = "/usr/share/maven"  // Update this path if Maven is installed elsewhere
    }

    stages {
        stage('Checkout') {
            agent { label 'slave1' }  // Checkout on slave1
            steps {
                echo "Checking out code on ${env.NODE_NAME}"
                git 'https://github.com/yourusername/your-maven-project.git'
            }
        }

        stage('Compile') {
            agent { label 'slave1' }  // Compile on slave1
            steps {
                echo "Compiling on ${env.NODE_NAME}"
                sh "${MAVEN_HOME}/bin/mvn clean compile"
            }
        }

        stage('Unit Tests') {
            agent { label 'slave2' }  // Run tests on slave2
            steps {
                echo "Running tests on ${env.NODE_NAME}"
                sh "${MAVEN_HOME}/bin/mvn test"
            }
        }

        stage('Package') {
            agent { label 'slave1' }  // Package on slave1
            steps {
                echo "Packaging on ${env.NODE_NAME}"
                sh "${MAVEN_HOME}/bin/mvn package"
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