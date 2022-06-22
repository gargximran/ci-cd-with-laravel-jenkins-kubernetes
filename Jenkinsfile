pipeline {
    agent any

    docker

    stages {
        stage("test") {
            steps {
                sh "ls"
                sh "docker -v"
            }
        }
    }
}