pipeline {
    agent any  // This means it can run on any available agent (node)

    environment {
        // Define environment variables (if needed)
        PYTHON_PATH = "${params.python}"
        JAVA_PATH = "${params.java}"
    }

    parameters {
        string(name: 'python', defaultValue: 'C:\\Path\\To\\Python', description: 'Path to Python executable')
        string(name: 'java', defaultValue: 'C:\\Path\\To\\Java', description: 'Path to Java executable')
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
                    // Ensure pip is installed and install dependencies
                    bat "pip install robotframework"
                    bat "pip install robotframework-seleniumlibrary"
                }
            }
        }

        stage('Run Tests') {
            steps {
                // Run the Robot Framework tests
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
