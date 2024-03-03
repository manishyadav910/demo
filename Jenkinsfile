pipeline {
    agent any

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
                    println "Branch Name : $branchName"

                    def userName = currentBuild.getBuildCauses()[0].userName
                    println "Username : $userName"
                    
                    def currentTime = new Date().toString()
                    println "CurrentTime : $currentTime"

                    def apiUrl = 'http://localhost:8084/saveDeploy'
                    def jsonBody = """
                    {
                    "id" : "1",
                    "env" : "UAT",
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
