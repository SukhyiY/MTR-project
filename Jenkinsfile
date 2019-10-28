pipeline {
    agent {
        docker {
            image 'node:6-alpine' 
            args '-p 3000:3000' 
        }
    }
    stages {
        stage('Install dependencies') { 
            steps {
                sh 'npm install' 
            }
        }
        stage('Test app') {
            steps {
                sh 'npm run test src/App.test.js'
            }
        }
        stage('Build app') {
            steps {
                sh 'npm run build'
            }
        }
    }
}
