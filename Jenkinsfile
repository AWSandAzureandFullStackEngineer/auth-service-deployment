pipeline {
    agent any

    stages {
        stage('Clone repository') {
          steps {
            checkout scm
          }    
       }
        stage('Update GIT') {
            steps {
                script {
                    catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                        withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                            //def encodedPassword = URLEncoder.encode("$GIT_PASSWORD",'UTF-8')
                            sh "git config user.email awsandazurecloudengineer@gmail.com"
                            sh "git config user.name AWSandAzureandFullStackEngineer"
                            //sh "git switch master"
                            sh "cat deployment.yml"
                            sh "sed -i 's+steven8519/authentication-service.*+steven8519/authentication-service:${DOCKERTAG}+g' deployment.yml"
                            sh "cat deployment.yml"
                            sh "git add ."
                            sh "git commit -m 'Done by Jenkins Job changemanifest: ${env.BUILD_NUMBER}'"
                            sh "git push https://${GIT_USERNAME}:@github.com/${GIT_USERNAME}/kubernetesmanifest.git HEAD:main"
                        }
                    }
                }
            }
        }
        stage('Clean up workspace') {
            steps {
                cleanWs()
            }
        }
    }
}
