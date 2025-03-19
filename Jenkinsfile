pipeline {
    agent any

    environment {
        // Define your Python version (adjust if needed)
        PYTHON_VERSION = 'python3'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: 'main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/mdanish6580/jenkins_pyest_pipeline.git']]])
            }
        }
        
        stage('Set up Python Environment') {
            steps {
                script {
                    sh "${PYTHON_VERSION} -m venv venv"  // Create virtual environment
                    sh ". venv/bin/activate"   // Activate the virtual environment
                    sh "venv/bin/pip install --upgrade pip"  // Upgrade pip within the venv
                    sh "venv/bin/pip install -r requirements.txt"  // Install dependencies
                }
            }
        }
        
        stage('Run Tests') {
            steps {
                script {
                    // Run the tests with pytest
                    sh ". venv/bin/activate && pytest pytest --no-header -v --maxfail=1 --disable-warnings -q --junitxml=test-results.xml"
                }
            }
        }

        stage('Post-build Actions') {
            steps {
                // Archive test results, e.g., JUnit XML format
                junit '**/test-results.xml'  // Jenkins will look for files starting with 'test-' and ending with '.xml'
            }
        }

    }
}