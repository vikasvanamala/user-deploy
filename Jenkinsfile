    pipeline {
        agent {
            node {
                label 'AGENT-1'
            }
        }
        environment {
            COURSE = "jenkins"
            appVersion = ""
            ACC_ID = "215446237872"
            PROJECT = "roboshop"
            COMPONENT = "user"
            REGION = "us-east-1"
        }
        options {
            timeout(time: 30, unit: 'MINUTES') 
            disableConcurrentBuilds()
        }
        parameters {
            string(name: 'appVersion', description: 'Which app version you want to deploy')
            choice(name: 'deploy_to', choices: ['dev', 'qa', 'prod'], description: 'Pick something')
        }
        stages {
            stage("deploy") {
                steps {
                    script {
                        withAWS(region:'us-east-1',credentials:'aws-creds'){
                            sh """
                                aws eks update-kubeconfig --region ${REGION} --name ${PROJECT}-${params.deploy_to}
                                kubectl get nodes
                            """
                        }
                    }
                }    
            }
        }
        
        post {
            always {
                echo 'i will always say hello/....'
                cleanWs()
            }
            success {
                echo 'I will run if success'
            }
            failure {
                echo 'I will run if failure'
            }
            aborted {
                echo 'pipeline is aborted'
            }
        }
    }

    