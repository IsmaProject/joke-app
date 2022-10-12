pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh "nppm install"
                sh "npm run build"
                sh "npm run test"
            }
        }
    }
}