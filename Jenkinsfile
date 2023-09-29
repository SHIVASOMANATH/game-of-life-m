pipeline {
    agent { label 'game'}
    options {
        retry(1)
        timeout(time: 2, unit: 'HOURS')
    }
    tools {
        maven 'Maven_3.6'
        jdk 'JAVA_8'
    }
    parameters {
        choice(name: 'GOAL', choices: ['package','clean package','install','clean install'],
         description: 'This is Maven Goal')
    }
    stages  {
        stage('source code') {
            steps {
                git url: 'https://github.com/nevergiveuptillu/spring-petclinic.git',
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
}