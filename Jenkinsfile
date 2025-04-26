pipeline {
    agent { label "Jenkins-slave" }
    environment {
              APP_NAME = "devops-eks-project"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
               steps {
                  git branch: 'main', credentialsId: 'git-token', url: 'https://github.com/Vignesh2064/gitops-register-app.git'
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
                   git config --global user.name "Vignesh2064"
                   git config --global user.email "Vignesh271297@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                //withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
               // withCredentials([gitUsernamePassword(credentialsId: 'git-token',  gitToolName: 'Default')]) {
                 withCredentials([usernamePassword(credentialsId: 'git-token', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                    sh 'git push https://$GIT_USERNAME:$GIT_PASSWORD@github.com/Vignesh2064/gitops-register-app.git main'
                // }
                //   sh "git push https://github.com/Vignesh2064/gitops-register-app.git main"
                }
            }
        }
      
    }
}
