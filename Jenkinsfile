pipeline {
    agent {label 'MAVEN_JDK8'}
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
                jdk 'JDK_8_UBUNTU'
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
    post {
        success {
            mail subject: "Jenkins Build of ${JOB_NAME} with id ${BUILD_ID} is success",
                body: "use this url ${BUILD_URL} for more info",
                to:'team-all-qt@qt.com',
                from:'devops@qt.com'
        }
        failure {
            mail subject: "Jenkins Build of ${JOB_NAME} with id ${BUILD_ID} is failure",
            body: "use this url ${BUILD_URL} for more info",
            to:"${GIT_AUTHOR_EMAIL}",
            from:'devops@qt.com'
        }
    }
}
