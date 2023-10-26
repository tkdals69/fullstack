pipeline{
    agent any

    environment {
        dockerHubRegistry = 'moonsungkim'
        dockerHubRegistryCredential = 'docker-hub'
        githubCredential = 'github_cred'
    }

    stages {
        stage('check out application git branch'){
            steps {
                checkout scm
            }
            post {
                failure {
                    echo 'repository checkout failure'
                }
                success {
                    echo 'repository checkout success'
                }
            }
        }

        stage('docker image build'){
            steps{
                sh "docker build -t ${dockerHubRegistry}/react:${currentBuild.number} ./frontend"
                sh "docker build -t ${dockerHubRegistry}/react:latest ./frontend"

                sh "docker build -t ${dockerHubRegistry}/nodejs:${currentBuild.number} ./backend"
                sh "docker build -t ${dockerHubRegistry}/nodejs:latest ./backend"

                sh "docker build -t ${dockerHubRegistry}/db:${currentBuild.number} ./mysql"
                sh "docker build -t ${dockerHubRegistry}/db:latest ./mysql"
            }
            post {
                    failure {
                      echo 'Docker image build failure !'
                    }
                    success {
                      echo 'Docker image build success !'
                    }
            }
        }
        stage('Docker Image Push') {
            steps {
                withDockerRegistry([ credentialsId: dockerHubRegistryCredential, url: "" ]) {
                    sh "docker push ${dockerHubRegistry}/react:${currentBuild.number}"
                    sh "docker push ${dockerHubRegistry}/react:latest"

                    sh "docker push ${dockerHubRegistry}/nodejs:${currentBuild.number}"
                    sh "docker push ${dockerHubRegistry}/nodejs:latest"

                    sh "docker push ${dockerHubRegistry}/db:${currentBuild.number}"
                    sh "docker push ${dockerHubRegistry}/db:latest"

                    sleep 10 /* Wait uploading */
                }
            }
            post {
                    failure {
                      echo 'Docker Image Push failure !'
                      sh "docker rmi ${dockerHubRegistry}/react:${currentBuild.number}"
                      sh "docker rmi ${dockerHubRegistry}/react:latest"

                      sh "docker rmi ${dockerHubRegistry}/nodejs:${currentBuild.number}"
                      sh "docker rmi ${dockerHubRegistry}/nodejs:latest"

                      sh "docker rmi ${dockerHubRegistry}/db:${currentBuild.number}"
                      sh "docker rmi ${dockerHubRegistry}/db:latest"
                    }
                    success {
                      echo 'Docker image push success !'
                      sh "docker rmi ${dockerHubRegistry}/react:${currentBuild.number}"
                      sh "docker rmi ${dockerHubRegistry}/react:latest"

                      sh "docker rmi ${dockerHubRegistry}/nodejs:${currentBuild.number}"
                      sh "docker rmi ${dockerHubRegistry}/nodejs:latest"

                      sh "docker rmi ${dockerHubRegistry}/db:${currentBuild.number}"
                      sh "docker rmi ${dockerHubRegistry}/db:latest"
                    }
            }
        }
        stage('K8S Manifest Update') {
            steps {
                sh "ls"
                sh 'mkdir -p gitOpsRepo'
                dir("gitOpsRepo")
                {
                    git branch: "main",
                    credentialsId: githubCredential,
                    url: 'https://github.com/moonstar0331/fullstack-app-k8s-manifest.git'

                    sh "git config --global user.email 'moonsung0331@gmail.com'"
                    sh "git config --global user.name 'moonstar0331'"

                    sh "sed -i 's/react:.*\$/react:${currentBuild.number}/g' web-deployment.yaml"
                    sh "sed -i 's/nodejs:.*\$/nodejs:${currentBuild.number}/g' api-deployment.yaml"
                    sh "sed -i 's/db:.*\$/mysql:${currentBuild.number}/g' mysql-deployment.yaml"

                    sh "git add ."
                    sh "git commit -m '[UPDATE] k8s ${currentBuild.number} image versioning'"
                    withCredentials([gitUsernamePassword(credentialsId: githubCredential,
                                     gitToolName: 'git-tool')]) {
                        sh "git remote set-url origin https://github.com/moonstar0331/fullstack-app-k8s-manifest"
                        sh "git push -u origin main"
                    }
                }
            }
            post {
                    failure {
                      echo 'K8S Manifest Update failure !'
                    }
                    success {
                      echo 'K8S Manifest Update success !'
                    }
            }
        }

    }
}