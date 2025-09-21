pipeline {
    agent {
        node {
            label 'My-Jenkins-Agent'
        }
    }
    //agent any
 
    environment {
        APP_NAME = "devops-03-pipeline--gitops"
    }



    stages {


     stage('Cleanup Workspace') {
            steps {
                script {
                    cleanWs()
                }
            }
        }


        stage('SCM GitHub') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/UlasGultekin/devops-03-pipeline--gitops']])
            }
        }

       stage("Update the Deployment Tags") {
    steps {
        sh """
           echo "Before update:"
           cat deployment.yaml

           sed -i "s|\\(image: *ulasgltkn/devops-03-pipeline:\\).*|\\1${IMAGE_TAG}|" deployment.yaml

           echo "After update:"
           cat deployment.yaml
        """
    }
}





      /*  stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "UlasGultekin"
                   git config --global user.email "gltknulas96@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                  sh "git push https://github.com/UlasGultekin/devops-03-pipeline--gitops main"
                }
            }
        }*/
stage("Push the changed deployment file to Git") {
  steps {
    withCredentials([usernamePassword(
      credentialsId: 'Github',             // Jenkins'teki ID (büyük/küçük harfe dikkat)
      usernameVariable: 'GIT_USER',
      passwordVariable: 'GIT_TOKEN'
    )]) {
      sh '''#!/usr/bin/env bash
                set -euo pipefail

                git config user.name  "UlasGultekin"
                git config user.email "gltknulas96@gmail.com"

# (opsiyonel) detached HEAD ise branch'e geç
                git checkout -B main origin/main || true

                git add deployment.yaml || true

# değişiklik yoksa fail etme
                if git diff --cached --quiet; then
                    echo "No changes to commit."
                exit 0
                fi

                git commit -m "Updated Deployment Manifest"

# Token'ı URL'de kullan; Jenkins maskeler
                git push "https://${GIT_USER}:${GIT_TOKEN}@github.com/UlasGultekin/devops-03-pipeline--gitops" HEAD:main
'''
    }
  }
}









    }
}