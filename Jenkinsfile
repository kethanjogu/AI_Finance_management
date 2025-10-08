pipeline {
    agent any
    environment {
        // Define the path to your Node.js installation
        // This path might be different on your Jenkins agent
        def NODE_JS_HOME = "/usr/local/bin" 
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
