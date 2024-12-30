pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'angular17-todo-app:latest'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/ahmedomri5020/angular17-todo-app.git'
            }
        }

        stage('Install nvm and Node.js') {
            steps {
                script {
                    echo 'Installing nvm and Node.js v18...'
                    sh '''
                        curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash
                        source ~/.nvm/nvm.sh
                        nvm install 18
                        nvm use 18
                    '''
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    echo 'Installing dependencies...'
                    sh 'npm install'
                }
            }
        }

        stage('Build Application') {
            steps {
                script {
                    echo 'Building Angular application...'
                    sh 'npm start -- --prod'
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    echo 'Running tests...'
                    sh 'npm run test -- --watch=false --browsers=ChromeHeadless'
                }
            }
        }

        stage('Archive Test Results') {
            steps {
                script {
                    echo 'Archiving test results...'
                    junit '**/test-results/**/*.xml'
                }
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    if (fileExists('Dockerfile')) {
                        echo 'Building Docker image...'
                        sh 'docker build -t ${DOCKER_IMAGE} .'
                    }
                }
            }
        }

        stage('Archive Build Artifacts') {
            steps {
                script {
                    echo 'Archiving build artifacts...'
                    archiveArtifacts allowEmptyArchive: true, artifacts: 'dist/**', fingerprint: true
                }
            }
        }

        stage('Cleanup') {
            steps {
                script {
                    if (fileExists('Dockerfile')) {
                        echo 'Cleaning up Docker images...'
                        sh 'docker rmi ${DOCKER_IMAGE}'
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution finished.'
        }
    }
}
