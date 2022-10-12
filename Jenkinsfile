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
                    docker.withRegistry('https://registry.heroku.com', 'herokuid') {
                        sh "docker buildx build --platform linux/amd64 -t ${tag}:latest -t ${tag}:${env.BUILD_ID} ."
                        sh "docker push ${tag}:latest"
                    }
                }
                withCredentials([usernamePassword(credentialsId: 'herokuid', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh "HEROKU_API_KEY=${PASSWORD} npx heroku container:release web --app=jenkinsappt"
                }
            }
        }
    }
}