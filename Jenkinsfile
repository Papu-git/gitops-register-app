pipeline {
    agent { label "Jenkins-Agent" }
    
    environment {
        APP_NAME = "register-app-pipeline"
        RELEASE = "1.0.0"
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}" // Ensure BUILD_NUMBER is set
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
            steps {
                git branch: 'dev', credentialsId: 'github', url: 'https://github.com/Papu-git/registration-app/'
            }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh """
                    echo 'Before update:'
                    cat deployment.yaml
                    
                    sed -i 's|image: ${APP_NAME}:.*|image: ${APP_NAME}:${IMAGE_TAG}|g' deployment.yaml
                    
                    echo 'After update:'
                    cat deployment.yaml
                """
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASS', usernameVariable: 'GIT_USER')]) {
                    sh """
                        git config --global user.name "Papu"
                        git config --global user.email "chandancoolboy10@gmail.com"
                        
                        git add deployment.yaml
                        git commit -m "Updated Deployment Manifest"
                        
                        git push https://${GIT_USER}:${GIT_PASS}@github.com/Papu-git/gitops-register-app.git dev
                    """
                }
            }
        }
    }
}
