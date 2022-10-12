pipeline {
    agent any

    parameters {
        choice(name: 'NODE_VERSION', choices: ['18', '17'], description: 'Changing the version of node')
    }

    environnement{
        tag = "?registry.heroku.com/jenkinsappt/web"
    }

    stages {
        stage('Build') {
            steps {
                nodejs(nodeJSInstallationName:'${params.NODE_VERSION}') {
                    sh 'node -v'
                    sh 'npm install'
                    sh 'npm run build'
                    sh "npm run test"
                }
            }
        }

        stage('deploy') {
            steps {
                script {
                    docker.withRegistry('registry.heroku.com', 'herokuid') {
                        def image = docker.build("${env.tag}")
                        image.push()
                    }
                }
            }
        }
    }
}