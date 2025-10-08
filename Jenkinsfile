pipeline {
    agent any
    tools {
        // Use the name of your configured NodeJS installation in Jenkins
        nodejs 'node' 
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
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
