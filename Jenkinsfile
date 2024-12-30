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
      

    post {
        always {
            // Clean up workspace to free up space
            cleanWs()
        }
    }
}
