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
            steps{
                script{
                    def branchName = env.BRANCH_NAME
                    println "Branch Name : $branchName"

                    def author = env.CHANGE_AUTHOR
                    println "author : $author"

                    def userName = currentBuild.getBuildCauses()[0].getUserName()
                    println "Username : $userName"

                    // def buildUser = env.BUILD_USER_ID
                    // println "Build User : $buildUser"
                    
                    def currentTime = new Date().toString()
                    println "CurrentTime : $currentTime"

                    // def date = env.TAG_DATE
                    // println "Tag Date Value : $date"
                }
            }
        }
    }
}
