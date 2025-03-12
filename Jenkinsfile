pipeline {
    agent any  // This means it can run on any available agent (node)

    environment {
        PYTHON_PATH = "${params.python}"
        JAVA_PATH = "${params.java}"
        ROBOT_OUTPUT_DIR = "C:\\Users\\erijon.IMBUS\\Desktop\\RBF-MATERIALS\\PC-DataBase-Project\\PC-DataBase\\Logs"  // Directory for saving logs
        ROBOT_OUTPUT_DIR_DB = "C:\\Users\\erijon.IMBUS\\Desktop\\RBF-MATERIALS\\PC-DataBase-Project\\PC-DataBase"  // Directory containing .robot files
    }

    parameters {
        string(name: 'python', defaultValue: 'C:\\Program Files\\Python313\\python.exe', description: 'Path to Python executable')
        string(name: 'java', defaultValue: 'C:\\Program Files\\Java\\jdk-17\\bin\\java.exe', description: 'Path to Java executable')
        
        // Parameter for selecting which .robot file to execute
        choice(name: 'ROBOT_TEST_FILE', 
               choices: getRobotTestFiles(), 
               description: 'Select the .robot test case to run') // Dropdown list of available .robot files
               
        string(name: 'tags', defaultValue: '', description: 'Comma separated list of tags to filter test cases (e.g., tag1,tag2)')
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
                    def testFile = params.ROBOT_TEST_FILE  // Get the selected .robot file
                    def testTags = params.tags  // Get the tags if any
                    
                    def command = "robot --outputdir ${ROBOT_OUTPUT_DIR} ${ROBOT_OUTPUT_DIR_DB}\\${testFile}"

                    // If tags are provided, add the --include option to filter tests
                    if (testTags) {
                        command += " --include ${testTags.replace(',', ' ')}"
                    }

                    // Run Robot Framework tests with the selected test file and filtered tags (if provided)
                    echo "Running command: ${command}" // Debugging output
                    bat command
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

// Function to get the list of .robot files in the specified directory
def getRobotTestFiles() {
    def robotFiles = []
    def directory = "C:\\Users\\erijon.IMBUS\\Desktop\\RBF-MATERIALS\\PC-DataBase-Project\\PC-DataBase"
    
    // List all .robot files in the directory
    def files = bat(script: "dir /b ${directory}/*.robot", returnStdout: true).trim().split("\n")
    
    // Populate the choice list with the file names
    for (def file : files) {
        robotFiles.add(file)
    }
    
    return robotFiles
}
