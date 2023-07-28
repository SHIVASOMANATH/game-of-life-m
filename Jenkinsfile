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
    stages {
        stage('source code') {
            steps {
                git url: 'https://github.com/SHIVASOMANATH/game-of-life-m.git',
                    branch: 'master'
            }
        }
        stage('package') {
            steps {
                sh script: 'mvn package'
            }
        }
        stage('report') {
            steps {
                junit testResults: '**/target/surefire-reports/TEST-*.xml'
                archiveArtifacts artifacts: '**/target/gameoflife.war'
            }
        }
    }
}