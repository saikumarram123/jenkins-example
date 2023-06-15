import groovy.transform.Field

pipeline {
    agent any

    stages {
        stage ('Compile Stage') {

            steps {
                withMaven(maven : 'maven_3_5_0') {
                    sh 'mvn clean compile'
                }
            }
        }

        stage ('Testing Stage') {

            steps {
                withMaven(maven : 'maven_3_5_0') {
                    sh 'mvn test'
                }
            }
        }


       /* stage ('Deployment Stage') {
            steps {
                withMaven(maven : 'maven_3_5_0') {
                    sh 'mvn deploy'
                }
            }
        } */
    }
        post {
            always {
                script {
                    def user = ''
                    currentBuild.result = 'FAILURE'
                    for (cause in currentBuild.causes) {
                        if (cause.class.toString().contains('UserIdCause')) {
                            user = cause.userName
                            break
                        }
                    }
                    slackSend(channel: '#testing', message: "Build triggered by ${user}")
                }
            }
     }    
}
