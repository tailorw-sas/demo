pipeline {
    agent any
     triggers {
        // Configuración del webhook
        GenericTrigger(
            genericVariables: [
                [key: 'pusher_email', expressionType: 'JSONPath', expression: '$.pusher.email'],
                [key: 'pusher_name', expressionType: 'JSONPath', expression: '$.pusher.name'],
                [key: 'commit_message', expressionType: 'JSONPath', expression: '$.head_commit.message']
            ],
            token: '1234567890',  // Token que usarás en GitHub
            causeString: 'Triggered on push by $pusher_name',
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
