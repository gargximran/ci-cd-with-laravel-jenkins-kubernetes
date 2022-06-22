pipeline {
    agent any
    stages {
        stage("test") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-cred') {
                        def dockerImage = docker.build('gargximran/laravelw:jenkins.${env.BUILD_NUMBER}')
                    }
                }
            }
        }
    }
}