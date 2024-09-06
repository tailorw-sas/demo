pipeline {
    agent any
    environment {
        GITHUB_TOKEN = credentials('TokenForJenkinsAuthentication') // El token guardado en Jenkins
    }
    stages {
        stage('Get PR Author Email') {
            steps {
                script {
                    // Número del Pull Request
                    def branchName = sh(script: 'git rev-parse --abbrev-ref HEAD', returnStdout: true).trim()
                    def prNumber = branchName.replaceAll('refs/pull/', '').replaceAll('/merge', '')
                    echo "Pull Request Number: ${prNumber}"

                    def changes = sh(script: 'git diff --name-only HEAD^ HEAD', returnStdout: true).trim().split('\n')
                        echo "changes: ${changes}"
                }
            }
        }
    }
}
