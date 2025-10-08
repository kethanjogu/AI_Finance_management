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
                sh 'npm install --legacy-peer-deps'
            }
        }
        stage('Setup Environment') {
            steps {
                withCredentials([
                    string(credentialsId: 'clerk-secret-key', variable: 'CLERK_SECRET_KEY'),
                    string(credentialsId: 'gemini-api-key', variable: 'GEMINI_API_KEY'),
                    string(credentialsId: 'resend-api-key', variable: 'RESEND_API_KEY'),
                    string(credentialsId: 'arcjet-key', variable: 'ARCJET_KEY'),
                    string(credentialsId: 'database-url', variable: 'DATABASE_URL'),
                    string(credentialsId: 'direct-url', variable: 'DIRECT_URL'),
                    string(credentialsId: 'clerk-publishable-key', variable: 'NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY')
                ]) {
                    sh '''
                        echo "NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=${NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY}" > .env.local
                        echo "CLERK_SECRET_KEY=${CLERK_SECRET_KEY}" >> .env.local
                        echo "DATABASE_URL=${DATABASE_URL}" >> .env.local
                        echo "DIRECT_URL=${DIRECT_URL}" >> .env.local
                        echo "GEMINI_API_KEY=${GEMINI_API_KEY}" >> .env.local
                        echo "RESEND_API_KEY=${RESEND_API_KEY}" >> .env.local
                        echo "ARCJET_KEY=${ARCJET_KEY}" >> .env.local
                        echo "NEXT_PUBLIC_CLERK_SIGN_IN_URL=/sign-in" >> .env.local
                        echo "NEXT_PUBLIC_CLERK_SIGN_UP_URL=/sign-up" >> .env.local
                        echo "NEXT_PUBLIC_CLERK_AFTER_SIGN_IN_URL=/onboarding" >> .env.local
                        echo "NEXT_PUBLIC_CLERK_AFTER_SIGN_UP_URL=/onboarding" >> .env.local
                    '''
                }
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
