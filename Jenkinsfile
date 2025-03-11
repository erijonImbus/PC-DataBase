pipeline {
    agent any  // This means it can run on any available agent (node)

    environment {
        PYTHON_PATH = "${params.python}"
        JAVA_PATH = "${params.java}"
        ROBOT_OUTPUT_DIR = "C:\\Users\\erijon.IMBUS\\Desktop\\RBF-MATERIALS\\PC-DataBase-Project\\PC-DataBase\\Logs"
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
                script {
                    // Run Robot Framework tests and save results in the specified output directory
                    bat "robot --outputdir ${ROBOT_OUTPUT_DIR} ComputerDataBase.robot"
                }
            }
        }

        stage('Publish Results') {
            steps {
                // Archive the output files (logs, reports, XML results)
                archiveArtifacts artifacts: "${ROBOT_OUTPUT_DIR}/*.xml, ${ROBOT_OUTPUT_DIR}/*.html", allowEmptyArchive: true
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
        always {
            cleanWs()  // Clean workspace after the pipeline completes
        }
    }
}
