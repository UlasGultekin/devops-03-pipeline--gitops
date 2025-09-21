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
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/UlasGultekin/devops-03-pipeline--gitops']])
            }
        }

         stage("Update the Deployment Tags") {
            steps {
                
                sh """
                   cat deployment.yaml

                   sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g'           deployment.yaml
               
                   cat deployment.yaml
                """
            }
        }




        stage("Push the changed deployment file to Git") {
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
        }







    }
}