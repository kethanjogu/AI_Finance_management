pipeline {
    agent any
    environment {
        // Correct path for Node.js installed via NVM
        def NODE_JS_HOME = "/Users/saikethan/.nvm/versions/node/v20.10.0/bin" 
        def PATH = "${NODE_JS_HOME}:${env.PATH}"
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Verify Node and NPM') {
            steps {
                sh 'node --version'
                sh 'npm --version'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }
        stage('Prepare Artifacts') {
            steps {
                sh 'echo Build successful > artifact.txt'
            }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: 'artifact.txt, .next/**, public/**', allowEmptyArchive: true
        }
    }
}
