pipeline {
    agent any
    tools {
        'org.jenkinsci.plugins.docker.commons.tools.DockerTool' 'null'
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