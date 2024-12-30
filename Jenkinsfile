pipeline {
    agent any
    
    environment {
        // Define any environment variables if necessary
        DOCKER_IMAGE = 'angular17-todo-app:latest'
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Clone the Git repository
                git branch: 'main', url: 'https://github.com/ahmedomri5020/angular17-todo-app.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    // Install npm dependencies
                    echo 'Installing dependencies...'
                    sh 'npm install'
                }
            }
        }

        stage('Build Application') {
            steps {
                script {
                    // Use npm start to build the Angular app
                    echo 'Building Angular application...'
                    sh 'npm start -- --prod'  // Pass --prod if you need production build
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Run unit tests with Angular CLI
                    echo 'Running tests...'
                    sh 'npm run test -- --watch=false --browsers=ChromeHeadless'
                }
            }
        }

        stage('Archive Test Results') {
            steps {
                script {
                    // Archive the test results (JUnit or HTML)
                    echo 'Archiving test results...'
                    junit '**/test-results/**/*.xml'
                }
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    // Optional: If you want to build a Docker image
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
                    // Archive build artifacts (like the dist/ folder)
                    echo 'Archiving build artifacts...'
                    archiveArtifacts allowEmptyArchive: true, artifacts: 'dist/**', fingerprint: true
                }
            }
        }

        stage('Cleanup') {
            steps {
                script {
                    // Clean up Docker images after build
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
            // Actions to take after the pipeline, like cleanup or notifications
            echo 'Pipeline execution finished.'
        }
    }
}
