pipeline {
    agent { label "Jenkins-Agent" }
    environment {
              APP_NAME = "spring-boot-cicd-pipeline"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
               steps {
                   git branch: 'main', credentialsId: 'GitHub_username_password', url: 'https://github.com/raghugr16/gitops-register-app.git'
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
                   git config --global user.name "raghugr16"
                   git config --global user.email "raghugr16@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'GitHub_username_password', gitToolName: 'Default')]) {
                  sh "git push https://github.com/raghugr16/gitops-register-app main"
                }
            }
        }
      
    }
}
