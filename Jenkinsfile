pipeline {
    agent {
      kubernetes {
        yamlFile 'jenkins-agent.yaml'
      }
    }

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
                            sh "git config user.email nguyenhoangminh1106@devopslab.com"
                            sh "git config user.name nguyenhoangminh1106"
                            //sh "git switch master"
                            sh "cat deployment.yaml"
                            sh "sed -i 's+registry-uat.fke.fptcloud.com/576bb055-bc8d-4b31-a36a-a454eaeb2921/test.*+registry-uat.fke.fptcloud.com/576bb055-bc8d-4b31-a36a-a454eaeb2921/test:${DOCKERTAG}+g' deployment.yaml"
                            sh "cat deployment.yaml"
                            sh "git add ."
                            sh "git commit -m 'Done by Jenkins Job changemanifest: ${env.BUILD_NUMBER}'"
                            sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/kubernetesmanifest.git HEAD:main"
                       }
                    }
                }
            }
       }
    }
}
