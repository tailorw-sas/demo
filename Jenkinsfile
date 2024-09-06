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
                    def prNumber = env.CHANGE_ID
                    //def repoOwner = env.ORG_NAME     // Reemplaza con tu organización o usa env.GITHUB_REPOSITORY_OWNERr
                    def repoOwner = 'tailorw-sas'
                    //def repoName = env.REPO_NAME     // Reemplaza con tu repositorio o usa env.GITHUB_REPOSITORY_NAME
                    def repoName = 'demo'

                    
                  
                    // URL de la API de GitHub para obtener el PR
                    def apiUrl = "https://api.github.com/repos/${repoOwner}/${repoName}/pulls/${prNumber}"

                    echo "prNumber: ${prNumber}"
                    echo "apiURLL: ${apiUrl}"

                  
                    // Hacer una llamada curl para obtener la información del PR
                    //def response = sh(script: "curl -H 'Authorization: token ${GITHUB_TOKEN}' -H 'Accept: application/vnd.github.v3+json' ${apiUrl}", returnStdout: true)

                    // Parsear el JSON para obtener el correo del autor (si está disponible)
                    //def prInfo = readJSON(text: response)
                    //def authorLogin = prInfo.user.login

                    // Hacer una llamada para obtener la información del usuario
                    //def userApiUrl = "https://api.github.com/users/${authorLogin}"
                    //def userResponse = sh(script: "curl -H 'Authorization: token ${GITHUB_TOKEN}' -H 'Accept: application/vnd.github.v3+json' ${userApiUrl}", returnStdout: true)

                    // Parsear la respuesta para obtener el correo
                    //def userInfo = readJSON(text: userResponse)
                    //def authorEmail = userInfo.email

                    //if (authorEmail) {
                        //echo "El correo del autor del Pull Request es: ${authorEmail}"
                    //} else {
                        //echo "No se pudo obtener el correo del autor."
                    //}
                }
            }
        }
    }
}
