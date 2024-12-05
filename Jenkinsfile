pipeline{
    agent {
        kubernetes {
            inheritFrom "nodejs"
        }
    }
    environment {
       APP_NAME_FRONTEND = "cicd-frontend"
    }
    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }
        stage("checkout from scm"){
            steps{
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/Nikhil007nsg/cd-frontend'
                
            } 
        }
        stage("Update the Deployment Tags") {
            steps {
                sh """
                   cat deployment.yaml
                   sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                   cat deployment.yaml
                """
            }
        }
         stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "nikhil007nsg"
                   git config --global user.email "nikhil007nsg@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                  sh "git push https://github.com/Nikhil007nsg/cicd-frontend main"
                }
            }
        }
      
    }
}
