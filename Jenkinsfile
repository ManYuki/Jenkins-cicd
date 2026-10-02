pipeline {
    agent any
    
    tools {
        // Tells Jenkins to use the NodeJS tool we will configure later
        nodejs 'NodeJS' 
    }

    stages {
        stage('Fetch Code') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                echo 'Installing dependencies...'
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                echo 'Running unit tests...'
                sh 'npm test'
            }
        }
        stage('Package / Artifact') {
            steps {
                echo 'Simulating deployment by packaging artifacts...'
                sh 'tar -czvf application.tar.gz package.json app.js'
                archiveArtifacts artifacts: 'application.tar.gz', fingerprint: true
            }
        }
    }
}
