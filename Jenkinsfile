pipeline {
    agent any
    environment {
        GH_TOKEN  = credentials('GITHUB_CREDENTIALS_ID')
        GOOGLE_APPLICATION_CREDENTIALS = credentials('webapp-operator')
    }
    stages {
        stage('Checkout Code') {
            steps {
                script {
                    // Checkout the 'main' branch explicitly
                    checkout([$class: 'GitSCM', 
                              branches: [[name: 'refs/heads/main']], 
                              doGenerateSubmoduleConfigurations: false, 
                              extensions: [], 
                              submoduleCfg: [], 
                              userRemoteConfigs: [[url: 'https://github.com/cyse7125-fall2023-group2/webapp-helm-chart', 
                              credentialsId: 'GITHUB_CREDENTIALS_ID']]])
                }
            }
        }

        
        // stage('Fetch GitHub Credentials') {
        //     steps {
        //         script {
        //             // Define credentials for GitHub
        //             withCredentials([usernamePassword(credentialsId: 'GITHUB_CREDENTIALS_ID', usernameVariable: 'githubUsername', passwordVariable: 'githubToken')]) {
        //                 git branch: 'main', credentialsId: 'GITHUB_CREDENTIALS_ID', url: 'https://github.com/cyse7125-fall2023-group2/webapp-helm-chart'        
        //             }
        //         }
        //     }
        // }

        stage('Helm vesion'){
            steps{
                sh """
                helm version
                """
            }
        }
        stage('Helm Lint') {
            steps {
                sh 'helm lint .'
            }
        }

        stage('Helm Template') {
            steps {
                sh 'helm template .'
            }
        }

        stage('release') {
            steps {
                script {
                    // Define credentials for GitHub
                    withCredentials([usernamePassword(credentialsId: 'GITHUB_CREDENTIALS_ID', usernameVariable: 'githubUsername', passwordVariable: 'githubToken')]) {
                      withEnv(["GH_TOKEN=${githubToken}"]){
                       sh """
                            npx semantic-release
                       """
                      }     
                    }
                }
            }
        }

        stage('Create Release') {
            steps {
                script {
                    // Use Semantic Release to determine the version
                    newVersion = sh(returnStdout: true, script: "git describe --abbrev=0 --tags | tr -d 'v' ").trim()


                    // Package Helm chart
                    sh "helm package . --version ${newVersion}"
                }
            }
        }

        stage('Create GitHub Release') {
            steps {
                script {
                    
                     withCredentials([usernamePassword(credentialsId: 'GITHUB_CREDENTIALS_ID', usernameVariable: 'githubUsername', passwordVariable: 'githubToken')]) {
                      withEnv(["GH_TOKEN=${githubToken}"]){
                    def chartYaml = readYaml(file: 'Chart.yaml')
                    def chartName = chartYaml.name
                    def zipFileName = "${chartName}-${newVersion}.tgz"
                    sh "ls"
                    def tagExists = sh(returnStatus: true, script: "gh release view ${newVersion} --repo cyse7125-fall2023-group2/webapp-helm-chart")
                    if (tagExists != 0) {
                        sh "gh release create ${newVersion} ${zipFileName}  --title 'Release ${newVersion}' --notes 'Release notes for version ${newVersion}' --target main" 
                    }
                    
                    // Create GitHub release using "gh" CLI
                    
                    }

                     }
            }
        }
        }

    }


}