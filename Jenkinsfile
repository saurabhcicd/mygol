pipeline {
    agent {label 'JDK_8'}
    triggers { pollSCM ('H/30 * * * *') }
    parameters {
        choice(name: 'MAVEN_GOAL', choices: ['package', 'install', 'clean'], description: 'MAVEN_GOAL') 
    }
    stages {
        stage('vcs') {
            steps {
                git url: 'https://github.com/saurabhdevops123/game-of-life.git',
                    branch: 'declarative'
                
            }
        }
        stage('package') {
            tools {
                jdk 'JDK_8'
            }
            steps {
                sh "mvn ${params.MAVEN_GOAL}"
            }
        }
        stage('post build') {
            steps {
                archiveArtifacts artifacts: '**/target/gameoflife.war',
                                 onlyIfSuccessful: true
                junit testResults: '**/surefire-reports/TEST-*.xml'
            }
        }
    }
}
