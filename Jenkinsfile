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
                    
                    def apiUrl = "https://api.github.com/repos/tailorw-sas/demo/pulls?state=all"
                    def response = sh(script: "curl -H \"Authorization: Bearer ${GITHUB_TOKEN}\" -H \"Accept: application/vnd.github+json\" -H \"X-GitHub-Api-Version: 2022-11-28\" ${apiUrl}", returnStdout: true)
                    echo "${response}"
                    def pullRequests = readJSON(text: response)
                    def prTitle
                    def prUser
                    def matchingPullRequest = pullRequests.find { pr -> pr.merge_commit_sha == env.GIT_COMMIT }
                    if (matchingPullRequest) {
                        echo "Found PR: ${matchingPullRequest.title}"
                        echo "PR Number: ${matchingPullRequest.number}"
                        echo "Author: ${matchingPullRequest.user.login}"
                    } else {
                        echo "No PR found with merge_commit_sha: ${gitCommitSha}"
                    }
                    
                    
                    def changes = sh(script: 'git diff --name-only HEAD^ HEAD', returnStdout: true).trim().split('\n')
                        echo "changes: ${changes}"
                }
            }
        }
    }
}
