pipeline {
    agent any  // This means it can run on any available agent (node)

    environment {
        PYTHON_PATH = "${params.python}"
        JAVA_PATH = "${params.java}"
    }

    parameters {
        string(name: 'python', defaultValue: 'C:\\Program Files\\Python313\\python.exe', description: 'Path to Python executable')
        string(name: 'java', defaultValue: 'C:\\Program Files\\Java\\jdk-17\\bin\\java.exe', description: 'Path to Java executable')
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from GitHub
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    bat "pip install robotframework"
                    bat "pip install robotframework-seleniumlibrary"
                }
            }
        }

        stage('Run Tests') {
            steps {
                bat "robot ComputerDataBase.robot"
            }
        }
    }

    post {
        success {
            echo 'Build completed successfully'
        }
        failure {
            echo 'Build failed'
        }
    }
}
