pipeline {
  agent any
  stages {
    stage('Deploy to kubernetes r') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
           script {
             def text = readFile file: "infra/deployment.yaml"
            text = text.replaceAll("%IMAGE_ONE%", "${env.dockerHubUser}/laravel-application-one-bs:jenkins-build-${env.BUILD_NUMBER}")
            writeFile file: "infra/deployment.yaml", text: text
           }          
          sh "cat infra/deployment.yaml"
            
        }
      }
    }
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

        withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
            sh "docker rmi ${env.dockerHubUser}/laravel-application-one-bs:jenkins.build.${env.BUILD_NUMBER}"
            sh "docker rmi ${env.dockerHubUser}/laravel-application-two-bs:jenkins.build.${env.BUILD_NUMBER}"
        }
        
      }
    }

    stage('Deploy to kubernetes') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
            sh 'sed "s/{{env.dockerHubUser}}/laravel-application-one-bs:jenkins.build.{{env.BUILD_NUMBER}}/IMAGE_ONE/g" infra/deployment.yaml'
            sh 'sed "s/{{env.dockerHubUser}}/laravel-application-two-bs:jenkins.build.{{env.BUILD_NUMBER}}/IMAGE_TWO/g" infra/deployment.yaml'

            sh "cat infra/deployment.yaml"
            
        }
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