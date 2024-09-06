pipeline {
    agent any

    environment {
        GITHUB_TOKEN = credentials('TokenForJenkinsAuthentication') // El token guardado en Jenkinss
    }
    stages {
        stage('Get PR Author Email') {
            steps {
                script {
                    
                    echo "****************Commit Hash: ${env.GIT_COMMIT}"
                     echo "****************Commit Id: ${env.CHANGE_ID}"
                    echo "****************Commit Title: ${env.CHANGE_TITLE}"
                     echo "****************Commit Author: ${env.CHANGE_AUTHOR}"
                    
                    def apiUrl = "https://api.github.com/repos/tailorw-sas/demo/pulls?state=all"
                    //def response = sh(script: "curl ")

                    
                    def changes = sh(script: 'git diff --name-only HEAD^ HEAD', returnStdout: true).trim().split('\n')
                        echo "changes: ${changes}"
                }
            }
        }
    }
}
