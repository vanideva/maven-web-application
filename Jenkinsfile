node
{
    def mavenHome = tool name: "maven 3.8.1"

    stage('GettingCodeFromGit') 
    {
    git branch: 'development', credentialsId: '199c5493-954d-4765-af1c-4bb5893593a6', url: 'https://github.com/vanideva/maven-web-application.git'
}
    stage('Build')
    {
        sh "${mavenHome}/bin/mvn clean package"
    }
    stage('GeneratingSonarQubeReport')
    {
        sh "${mavenHome}/bin/mvn sonar:sonar"
    }
    /* stage('Quality Gate')
        {
            timeout(time: 1, unit: 'HOURS'){
                def qg = waitForQualityGate()
                if (qg.status != 'OK'){
                    error "Pipeline aborted due to quality gate failure: ${qg.status}"
                }
            }
        }
        */
    stage('EmailNotification')
    {
    emailext body: '''Build Over



Regards
Sreevani.P''', subject: 'Build Over', to: 'sreevaniy92@gmail.com'
}
    stage('DiscardingOldBuilds')
    {
        properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '3', daysToKeepStr: '', numToKeepStr: '3')), [$class: 'JobLocalConfiguration', changeReasonComment: '']])
    
    }
    stage('PollSCM')
    {
        properties([[$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
    }
}
