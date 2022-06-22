pipeline {
    agent {
        docker { image "docker' }
    }
    
    stages {
        stage("test") {
            steps {
                sh "docker -v"
            }
        }
    }
}