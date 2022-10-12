pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                nodejs(nodeJSInstallationName: '18.0') {
                    sh 'npm install'
                    sh 'npm run build'
                    sh "npm run test"
                }
            }
        }
    }
}