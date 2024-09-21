pipeline {
    agent {
        label "jenkins-agent"
    }
    environment {
        APP_NAME = "java-homework003-pipeline"
    }

    stages {
        stage ("Clean up workspace"){
            steps {
                clearWs()
            }
        }
        stage ("Checkout from SCM"){
            steps {
                git branch: "main", credentialsId: "github", url : "https://github.com/thanak81/spring-argo-pipeline"
            }
        }
        stage ("Update the Deployment Tags"){
            steps {
                script {
                sh """
                    cat k8s/deployment.yaml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g k8s/deployment.yaml'
                    cat k8s/deployment.yaml
                """
                }
              
            }
        }
        stage ("Push the changed deployment file to GIT"){
            steps{
                script {
                sh """
                    git config --global user.name "thanak81"
                    git config --global user.email "thanakmech@gmail.com"
                    git add k8s/deployment.yaml
                    git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword[credentialsId: "github", gitToolName: "Default"]]){
                    sh "git push https://github.com/thanak81/spring-argo-pipeline main"
                }
                }
               
            }
        }
    }
}