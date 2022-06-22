pipeline {
  agent any
  stages {    
    stage('Deploy to kubernetes fd') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
            script {
              def text = readFile file: "infra/deployment.yaml"
              text = text.replaceAll("IMAGE_ONE", "${env.dockerHubUser}/laravel-application-one-bs:jenkins.build.${env.BUILD_NUMBER}")
              text = text.replaceAll("IMAGE_TWO", "${env.dockerHubUser}/laravel-application-two-bs:jenkins.build.${env.BUILD_NUMBER}")
              writeFile file: "infra/deployment.yaml", text: text
            }  
        }

        sshagent(credentials : ['kubernetes_control_plane_cred']) {
            sh 'ssh -o StrictHostKeyChecking=no ubuntu@3.86.114.18 "mkdir -p kubernetes/jenkins"'            
            sh 'scp infra/deployment.yaml ubuntu@3.86.114.18:kubernetes/jenkins/deployment.yaml'
            sh 'ssh -o StrictHostKeyChecking=no ubuntu@3.86.114.18 "kubectl apply -f kubernetes/jenkins/deployment.yaml"' 
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
            script {
              def text = readFile file: "infra/deployment.yaml"
              text = text.replaceAll("IMAGE_ONE", "${env.dockerHubUser}/laravel-application-one-bs:jenkins.build.${env.BUILD_NUMBER}")
              text = text.replaceAll("IMAGE_TWO", "${env.dockerHubUser}/laravel-application-two-bs:jenkins.build.${env.BUILD_NUMBER}")
              writeFile file: "infra/deployment.yaml", text: text
            }  
        }

        sshagent(credentials : ['kubernetes_control_plane_cred']) {
            sh 'ssh -o StrictHostKeyChecking=no ubuntu@$env.kubectl_client_server_ip "mkdir -p kubernetes/jenkins"'            
            sh 'scp infra/deployment.yaml ubuntu@$env.kubectl_client_server_ip:kubernetes/jenkins/deployment.yaml'
            sh 'ssh -o StrictHostKeyChecking=no ubuntu@$env.kubectl_client_server_ip "kubectl apply -f kubernetes/jenkins/deployment.yaml' 
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