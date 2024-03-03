pipeline {
    agent any

    parameters {
        choice(name: 'ENVIRONMENT', choices: ['PROD', 'UAT', 'DEV'], description: 'Environment to deploy')
        booleanParam(name: 'SUCCESS_BUILD', defaultValue: true, description: 'Perform a build SUCCCESSFULLY')
    }

    stages{
        stage('Build'){
            steps{
                echo 'Building...'
            }
        }
        stage('Deploy'){
            steps{
                echo 'Deploying...'
            }
        }
        stage('Post-Build'){
            when {
                expression { currentBuild.resultIsBetterOrEqualTo('SUCCESS') }
            }
            steps{
                script{
                    def branchName = env.BRANCH_NAME
                    def userName = currentBuild.getBuildCauses()[0].userName
                    def currentTime = new Date().toString()
                    def apiUrl = 'http://localhost:8084/saveDeploy'
                    def jsonBody = """
                    {
                    "id" : "1",
                    "env" : "$params.ENVIRONMENT",
                    "branch" : "$branchName",
                    "user" : "$userName",
                    "time" : "$currentTime" 
                    }
                    """
                    def curlCommand = "curl -X POST -H 'Content-Type: application/json' -d '${jsonBody}' ${apiUrl}"
                    sh(curlCommand)
                }
            }
        }
    }
}
