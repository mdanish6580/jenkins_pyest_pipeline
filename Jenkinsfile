pipeline {
    agent any

    environment {
        // Define your Python version (adjust if needed)
        PYTHON_VERSION = 'python3.10'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout your source code from version control
                checkout scm
            }
        }
        
        stage('Set up Python Environment') {
            steps {
                script {
                    // Set up Python environment
                    sh "${PYTHON_VERSION} -m venv venv"   // Create a virtual environment
                    sh ". venv/bin/activate"              // Activate the virtual environment
                    sh "pip install --upgrade pip"        // Upgrade pip
                    sh "pip install -r requirements.txt"  // Install dependencies (ensure you have a requirements.txt)
                }
            }
        }
        
        stage('Run Tests') {
            steps {
                script {
                    // Run the tests with pytest
                    sh ". venv/bin/activate && pytest --maxfail=1 --disable-warnings -q"
                }
            }
        }

        stage('Post-build Actions') {
            steps {
                // Archive test results, e.g., JUnit XML format
                junit '**/test-*.xml'  // Jenkins will look for files starting with 'test-' and ending with '.xml'
            }
        }
    }

    post {
        always {
            // Clean up the environment (optional)
            cleanWs()
        }
    }
}
