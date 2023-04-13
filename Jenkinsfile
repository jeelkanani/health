

pipeline{
    agent any
    options{
        buildDiscarder(logRotator(numToKeepStr: '5', daysToKeepStr: '5'))
        timestamps()
    }
    environment{
        
        // registry = "<dockerhub-username>/<repo-name>"
        // registryCredential = '<dockerhub-credential-name>'        
    }
    
    stages{
       stage('Building image') {
      steps{
        script {
          withCredentials([usernamePassword(credentialsId: 'docker', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]){
            sh "docker build -t ${USERNAME}/helthtracker  ."
            sh "echo ${PASSWORD} | docker login -u ${USERNAME} --password-stdin"
            sh "docker push ${USERNAME}/helthtracker"
        }
      }
    }
       stage('Deploy Image') {
      steps{
         script {
            withCredentials([usernamePassword(credentialsId: 'docker', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]){
            sh "docker run -d --name HelthTracker -p 3000:3000 -p 3001:3001 ${USERNAME}/helthtracker"
            sh "docker image prune -a -f"
        }
        }
      }
    }
}
}
}