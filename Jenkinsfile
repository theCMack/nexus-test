pipeline {
    agent {
        docker { image 'node:8' }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'npm install' 
            }
        }
    }
}