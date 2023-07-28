pipeline {
    agent { label 'JDK_8' }
    options {
         retry(3)
         timeout(time: 2, unit: 'HOURS')   
    }
    triggers {
        pollSCM('* * * * *')    
    }
    tools {
        maven 'MAVEN_DEFAULT'
        jdk 'JDK_8'
    }
    parameters {
        choice(name: 'GOAL', choices: ['package','clean package','install','clean install'],
         description: 'This is Maven Goal')
    }
    stages {
        stage('source code') {
            steps {
                git url: 'https://github.com/SHIVASOMANATH/game-of-life-m.git',
                    branch: 'master'
            }
        }
        stage('package') {
            steps {
                sh script: "mvn ${params.GOAL}"
            }
        }
        stage('report') {
            steps {
                junit testResults: '**/target/surefire-reports/TEST-*.xml'
                archiveArtifacts artifacts: '**/target/gameoflife.war'
            }
        }
    }
    post {
        success {
            mail subject: "${JOB_NAME}: Has Completed Success",
                 body: "working \n Build Url ${BUILD_URL}",
                 to: "all@qts.com"
        }
        failure {
            mail subject: "${JOB_NAME}: Has Completed with Failed",
                 body: "not working \n Build Url ${BUILD_URL}",
                 to: "all@qts.com"    
        }
    }
}