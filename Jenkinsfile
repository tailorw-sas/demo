@Library(['PullRequestInfo', 'emailNotification']) _
import com.tailorwsas.notificaciones.NotificadorEmail

pipeline {
    agent any

    environment {
        SISTEMA = 'ERP'
        GITHUB_REPO = 'demo'
        GIT_TOKEN = "${env.GITHUB_TOKEN}"
        EMAIL_FROM = "${env.CORREO_ORIGEN}"
        ESTADO_OK = 'OK'
        ESTADO_ERROR = 'ERROR'
        ORIGIN_BRANCH_NAME = "${env.GIT_BRANCH}"
    }
    stages {
        stage('Get PR Author Email') {
            steps {
                script {
                    
                    echo "****************Commit Hash: ${env.GIT_COMMIT} ****************"
                    
                    def apiUrl = "https://api.github.com/repos/tailorw-sas/demo/pulls?state=all"
                    def response = sh(script: "curl -H \"Authorization: token ${env.GIT_TOKEN}\" -H 'Accept: application/vnd.github.v3+json' ${apiUrl}", returnStdout: true)
                    //echo "${response}"
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
                    
                    echo "*********** INVOCANDO A LA LIBRERIA: **************"
                    def prInfo = pullrequests.getPullRequestInfoByGitCommit(env.SISTEMA, env.GITHUB_REPO, env.GIT_COMMIT,  env.GIT_TOKEN)
                    echo "title: ${prInfo.title}"
                    echo "author: ${prInfo.author}"
                    echo "authorEmail: ${prInfo.authorEmail}"

                    env.PR_TITLE = prInfo.title
                    env.PR_AUTHOR = prInfo.author
                    
                    env.EMAIL_LIST = env.EMAIL_LIST_DEV_ERP
                    env.EMAIL_AUTHOR = prInfo.authorEmail
                    def branch = env.ORIGIN_BRANCH_NAME.replace('origin/', '')
                    env.BRANCH_NAME = branch

                    echo "*********** CHANGES: **************"
                    def changes = sh(script: 'git diff --name-only HEAD^ HEAD', returnStdout: true).trim().split('\n')
                        echo "changes: ${changes}"
                }
            }
        }
    }

    post {
        success {
            script {
                def notificator = new NotificadorEmail(this)
                notificator.sendEmail(env.EMAIL_AUTHOR, env.EMAIL_LIST, env.EMAIL_FROM, env.GITHUB_REPO, env.SISTEMA, env.BRANCH_NAME, env.ESTADO_OK, env.PR_TITLE, env.PR_AUTHOR)
            }
        }
        failure {
            script {
                def notificator = new NotificadorEmail(this)
                notificator.sendEmail(env.EMAIL_AUTHOR, env.EMAIL_LIST, env.EMAIL_FROM, env.GITHUB_REPO, env.SISTEMA, env.BRANCH_NAME, env.ESTADO_ERROR, env.PR_TITLE, env.PR_AUTHOR)
            }
        }
    }
}
