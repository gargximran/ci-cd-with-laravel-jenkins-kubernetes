pipeline {
    agent any
    tools {
        docker
    }
    stages {
        stage("test") {
            steps {
                sh "ls"
                sh "docker -v"
            }
        }
    }
}