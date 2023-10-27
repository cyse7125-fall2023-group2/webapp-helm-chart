ipeline {
    agent any
    stages {
        stage('Fetch GitHub Credentials') {
            steps {
                script {
                    // Define credentials for GitHub
                    withCredentials([usernamePassword(credentialsId: 'GITHUB_CREDENTIALS_ID', usernameVariable: 'githubUsername', passwordVariable: 'githubToken')]) {
                        git branch: 'main', credentialsId: 'GITHUB_CREDENTIALS_ID', url: 'https://github.com/cyse7125-fall2023-group2/webapp-helm-chart'        
                    }
                }
            }
        }

        stage('Helm vesion'){
            steps{
                sh """
                helm version
                """
            }
        }
        // stage('Helm Lint') {
        //     steps {
        //         sh 'helm lint path/to/your/chart'
        //     }
        // }

        // stage('Helm Template') {
        //     steps {
        //         sh 'helm template path/to/your/chart'
        //     }
        // }

        // stage('Create Release') {
        //     steps {
        //         script {
        //             // Use Semantic Release to determine the version
        //             def newVersion = sh(script: 'semantic-release-command', returnStatus: true).trim()

        //             // Update Chart.yaml with the new version
        //             sh "sed -i 's/^version: .*/version: ${newVersion}/' path/to/your/chart/Chart.yaml"

        //             // Package Helm chart
        //             sh "helm package path/to/your/chart -d ."
        //         }
        //     }
        // }



}