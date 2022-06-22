pipeline {
  agent any
  stages {
    stage('Docker Build Application One') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
          sh "docker build -f app1/build/Dockerfile -t ${env.dockerHubUser}/laravel-application-one-bs:jenkins.build.${env.BUILD_NUMBER} ./app1/src/"
        }
      }
    }

    stage('Docker Build Application Two') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
          sh "docker build -f app2/build/Dockerfile -t ${env.dockerHubUser}/laravel-application-two-bs:jenkins.build.${env.BUILD_NUMBER} ./app2/src/"
        }
      }
    }
    stage('Docker Push Application One') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
          sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
          sh "docker push ${env.dockerHubUser}/laravel-application-one-bs:jenkins.build.${env.BUILD_NUMBER}"
        }
      }
    }

    stage('Docker Push Application Two') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
          sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
          sh "docker push ${env.dockerHubUser}/laravel-application-two-bs:jenkins.build.${env.BUILD_NUMBER}"
        }
      }
    }

    stage('Docker Remove Images') {
      steps {
        sh "docker rmi ${env.dockerHubUser}/laravel-application-one-bs:jenkins.build.${env.BUILD_NUMBER}"
        sh "docker rmi ${env.dockerHubUser}/laravel-application-two-bs:jenkins.build.${env.BUILD_NUMBER}"
      }
    }
    
}
post {
    success {
      sh "echo 'Pipeline is successfully completed.'"
    }
    failure {
      sh" echo 'Pipeline failed. Please check the logs.'"
    }
}
}