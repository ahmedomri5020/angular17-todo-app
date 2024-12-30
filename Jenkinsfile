pipeline {
    agent any

    environment {
        NVM_DIR = "$HOME/.nvm"
    }

    stages {
        stage('Install nvm and Node.js') {
            steps {
                script {
                    // Install NVM
                    sh 'curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash'

                    // Load NVM into the shell session
                    sh """
                        export NVM_DIR=\$HOME/.nvm
                        [ -s "\$NVM_DIR/nvm.sh" ] && . "\$NVM_DIR/nvm.sh"  # This loads nvm
                        nvm install 18.20.5  # Installing the required Node.js version
                        nvm use 18.20.5
                    """
                }
            }
        }
        
        stage('Install Dependencies') {
            steps {
                script {
                    // Ensure that the correct version of Node is being used
                    sh """
                        export NVM_DIR=\$HOME/.nvm
                        [ -s "\$NVM_DIR/nvm.sh" ] && . "\$NVM_DIR/nvm.sh"  # This loads nvm
                        nvm use 18.20.5  # Ensure we are using the correct Node.js version
                        npm install  # Install project dependencies
                    """
                }
            }
        }

        stage('Build Application') {
            steps {
                script {
                    sh """
                        export NVM_DIR=\$HOME/.nvm
                        [ -s "\$NVM_DIR/nvm.sh" ] && . "\$NVM_DIR/nvm.sh"
                        nvm use 18.20.5
                        npm run build  # Build the application
                    """
                }
            }
        }

        stage('Build with Docker (Optional)') {
            when {
                expression { fileExists('Dockerfile') }
            }
            steps {
                script {
                    sh 'docker build -t my-app:latest .'
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    sh """
                        export NVM_DIR=\$HOME/.nvm
                        [ -s "\$NVM_DIR/nvm.sh" ] && . "\$NVM_DIR/nvm.sh"
                        nvm use 18.20.5
                        npm test  # Run automated tests
                    """
                }
            }
        }

        stage('Archive Test Results') {
            steps {
                script {
                    junit 'test-results/**/*.xml'  // Archive JUnit test results
                }
            }
        }

        stage('Archive Build Artifacts') {
            steps {
                archiveArtifacts artifacts: 'dist/**', fingerprint: true  // Archive build files
            }
        }
    }

    post {
        always {
            // Clean up workspace to free up space
            cleanWs()
        }
    }
}
