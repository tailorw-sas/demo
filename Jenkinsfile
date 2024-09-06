pipeline {
    agent any

    triggers {
        GenericTrigger(
            genericVariables: [
                [key: 'commit_message', expressionType: 'JSONPath', expression: '$.head_commit.message']
            ],
            token: '1234567890',
            causeString: 'Triggered by push',
            printContributedVariables: true,
            printPostContent: true
        )
    }
    
    environment {
        GITHUB_TOKEN = credentials('TokenForJenkinsAuthentication') // El token guardado en Jenkinss
    }
    stages {
        stage('Get PR Author Email') {
            steps {
                script {
                    // Número del Pull Request
                    def commitMessage = sh(script: 'git log -1 --pretty=%B', returnStdout: true).trim()
                    echo "Commit Message: ${commitMessage}"
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
