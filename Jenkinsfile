pipeline {
    agent any

    parameters {
        choice(name: 'ENVIRONMENT', choices: ['PROD', 'UAT', 'QA', 'DEV'], description: 'Environment to deploy')
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
                 if (!env.SUCCESS_BUILD) {
                        error("Failing the stage due to a condition....")
                    }
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
                    def id

                    switch (params.ENVIRONMENT) {
                        case 'PROD':
                            id = 1
                            break
                        case 'UAT':
                            id = 2
                            break
                        case 'QA':
                            id = 3
                            break
                        case 'DEV':
                            id = 4
                            break
                    }
                    
                    def apiUrl = 'http://localhost:8084/saveDeploy'
                    def jsonBody = """
                    {
                    "id" : "$id",
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
