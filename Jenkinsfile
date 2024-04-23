pipeline {
    agent none

    environment {
        AUTHOR = "Ramzy Rashaun Arief"
        EMAIL = "ramzyrash@gmail.com"
        WEB = "https://www.ramzyrash.com"
    }

    // triggers {
    //     cron("*/5 * * * *")
    //     // pollSCM("*/5 * * * *")
    //     // upstream(upstreamProjects: 'job1,job2', threshold: hudson.model.Result.SUCCESS)
    // }

    parameters {
        string(name: "NAME", defaultValue: "Guest", description: "What is your name?")
        text(name: "DESCRIPTION", defaultValue: "Guest", description: "Tell me about you")
        booleanParam(name: "DEPLOY", defaultValue: false, description: "Need to deploy?")
        choice(name: "SOCIAL_MEDIA", choices: ['Instagram', 'Facebook', 'Twitter'], description: "Which Social Media?")
        password(name: "SECRET", defaultValue: "", description: "Encrypt Key")
    }

    options {
        disableConcurrentBuilds()
        timeout(time: 10, unit: 'MINUTES')
    }
    
    stages {

        stage("Parameter") {
            agent {
                node {
                    label "linux && java11"
                }
            }
            steps {
                echo "Hello ${params.NAME}!"
                echo "You're description is ${params.DESCRIPTION}!"
                echo "Your social media is ${params.SOCIAL_MEDIA}"
                echo "Need to deploy : ${params.DEPLOY} to deploy!"
                echo "Your secret is ${params.SECRET}"
            }
        }

        stage('Prepare') {
            environment {
                APP = credentials("ramzy_rahasia")
            }

            agent {
                node {
                    label "linux && java11"
                }
            }

            steps {
                echo("Author : ${AUTHOR}")
                echo("Email : ${EMAIL}")
                echo("Web : ${WEB}")
                echo("Start Job : ${env.JOB_NAME}")
                echo("Start Build : ${env.BUILD_NUMBER}")
                echo("Branch Name : ${env.BRANCH_NAME}")
                echo("App User : ${APP_USR}")
                echo("App Password : ${APP_PSW}")
                sh('echo "App Password: $APP_PSW" > "rahasia.txt"')
            }
        }

        stage('Build') {
            agent {
                node {
                    label "linux && java11"
                }
            }

            steps {

                script {
                    for (int i = 0; i < 10; i++) {
                        echo("Script ${i}")
                    }
                }

                echo("Start Build")
                sh("./mvnw clean compile test-compile")
                echo("Finish Build")
            }
        }

        stage('Test') {
            agent {
                node {
                    label "linux && java11"
                }
            }

            steps {

                script {
                    def data = [
                        "firstName": "Ramzy",
                        "lastName": "Rashaun"
                    ]
                    writeJSON(file: "data.json", json: data)
                }

                echo("Start Test")
                sh("./mvnw test")
                echo("Finish Test")
            }
        }

        stage('Deploy') {
            input {
                message "Can we deploy"
                ok "Yes, of course"
                submitter "masterambi"
                parameters {
                    choice(name: "TARGET_ENV", choices: ['DEV', 'QA', 'PROD'], description: "Which Environment?")
                }
            }

            agent {
                node {
                    label "linux && java11"
                }
            }

            steps {
                echo "Deploy to ${TARGET_ENV}"
            }
        }
    }

    post {
        always {
            echo "I will always say Hello again!"
        }
        success {
            echo "Yay, success"
        }
        failure {
            echo "Oh no, failure"
        }
        cleanup {
            echo "Don't care success or error"
        }
    }
}