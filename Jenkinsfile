pipeline {
    agent any

    environment {
        GIT_TOKEN = "${env.GITHUB_TOKEN}"
    }
    stages {
        stage('Get PR Author Email') {
            steps {
                script {
                    
                    echo "****************Commit Hash: ${env.GIT_COMMIT} ****************"
                    
                    def apiUrl = "https://api.github.com/repos/tailorw-sas/demo/pulls?state=all"
                    def response = sh(script: "curl -H \"Authorization: token ${env.GIT_TOKEN}\" -H 'Accept: application/vnd.github.v3+json' ${apiUrl}", returnStdout: true)
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
