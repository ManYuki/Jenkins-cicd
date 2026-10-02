pipeline {
    agent any
    
    options {
        // RETENTION POLICY: Keep only the last 5 builds and artifacts to manage disk space
        buildDiscarder(logRotator(numToKeepStr: '5', artifactNumToKeepStr: '5'))
    }

    triggers {
        // TRIGGER AUTOMATION: Poll the GitHub repository every minute for new commits
        pollSCM('* * * * *') 
    }
    
    tools {
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
        stage('Package Artifact') {
            steps {
                echo 'Archiving code...'
                sh 'tar -czvf application.tar.gz package.json app.js Dockerfile'
                archiveArtifacts artifacts: 'application.tar.gz', fingerprint: true
            }
        }
        stage('Docker Build & Secure Login') {
            steps {
                echo 'Authenticating and building Docker image...'
                // CREDENTIAL BINDING: Securely injects credentials without exposing them in logs
                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    sh 'echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin'
                    sh 'docker build -t my-node-app:latest .'
                }
            }
        }
    }
    
    post {
        always {
            // CLEANUP STAGE: Wipes the workspace after the run to free up disk space
            cleanWs()
            echo 'Workspace cleaned successfully.'
        }
    }
}
