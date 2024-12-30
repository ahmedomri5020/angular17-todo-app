pipeline {
    agent any

    environment {
        NVM_DIR = "${HOME}/.nvm"
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }
        stage('Install nvm and Node.js') {
            steps {
                script {
                    echo 'Installing nvm and Node.js v18...'

                    // Install nvm and use Node.js v18
                    sh '''#!/bin/bash
                    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash
                    export NVM_DIR="$HOME/.nvm"
                    [ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"  # This loads nvm
                    nvm install 18
                    nvm use 18
                    '''
                }
            }
        }
        stage('Verify Node.js Version') {
            steps {
                script {
                    echo 'Verifying Node.js version'
                    sh 'node -v'
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Build Application') {
            steps {
                sh 'npm run build --prod'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'npm test'
            }
        }
        stage('Docker Build') {
            steps {
                echo 'Docker build step'
            }
        }
        stage('Archive Build Artifacts') {
            steps {
                echo 'Archiving build artifacts'
            }
        }
    }

    post {
        always {
            echo 'Cleaning up workspace'
            cleanWs()
        }
    }
}
