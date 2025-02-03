pipeline {
    agent any  // Run on any available agent

    environment {
        // Define environment variables
        VENV_DIR = "${WORKSPACE}/venv"
        PYTHON = "C:\\Users\\user\\AppData\\Local\\Programs\\Python\\Python312"  // Use python3 for Linux/macOS, or "python" for Windows
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out the code...'
                git 'https://github.com/VardanKhublaryan/AutomationExersice'  // Replace with your repo URL
            }
        }

        stage('Set Up Virtual Environment') {
            steps {
                echo 'Setting up a Python virtual environment...'
                bat  """
                    ${PYTHON} -m venv ${VENV_DIR}

                    // call ${VENV_DIR}\\Scripts\\activate  // For Windows
                    pip install --upgrade pip
                """
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing dependencies...'
                bat  """

                    // call ${VENV_DIR}\\Scripts\\activate  // For Windows
                    pip install -r requirements.txt
                """
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running tests...'
                bat  """

                    // call ${VENV_DIR}\\Scripts\\activate  // For Windows
                    pytest --junitxml=test-results.xml --html=report.html
                """
            }
        }

        stage('Publish Test Results') {
            steps {
                echo 'Publishing test results...'
                junit 'test-results.xml'  // Publish JUnit test results
                publishHTML(target: [
                    allowMissing: false,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: '.',
                    reportFiles: 'report.html',
                    reportName: 'HTML Test Report'
                ])
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            bat  """
                rm -rf ${VENV_DIR}  // Clean up the virtual environment
            """
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}