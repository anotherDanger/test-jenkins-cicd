pipeline{
    agent none
    environment{
            AUTHOR = "andhika danger"
            APP = credentials("app")
        }
    // triggers{
    //     // cron('* * * * *')
    //     pollSCM('* * * * *')
    // }
   stages{
            stage('Information'){
                agent{
                    node{
                        label 'golang && almalinux'
                    }
                }

                steps{
                    echo("Author: ${AUTHOR}")
                    echo("App: ${APP_USR}")
                    sh('echo "APP PSW: $APP_PSW" > "rahasia.txt"')
                    echo("Job Name: ${env.JOB_NAME}")
                    echo("Node Labels: ${env.NODE_LABELS}")
                    echo("Branch Name: ${env.BRANCH_NAME}")
                }
            }
            // stage('Preparation'){
            //     matrix{
            //         agent{
            //             node{
            //                 label 'golang && almalinux'
            //             }
            //         }
            //         axes{
            //             axis{
            //                 name 'OS'
            //                 values 'linux', 'windows', 'mac'
            //             }

            //             axis{
            //                 name 'ARC'
            //                 values '32', '64'
            //             }
            //         }
            //         stages{
            //             stage('Platform'){
            //                 steps{
            //                     echo("Run on ${OS} with ARC ${ARC}")
            //                 }
            //             }
            //         }
            //     }
            // }
            stage('Build'){
                agent{
                    node{
                        label 'golang && almalinux'
                    }
                }
                steps{
                    sh 'go build -o main'
                }
            }
            stage('Test'){
                agent {
                    node{
                        label 'golang && almalinux'
                    }
                }
                steps{
                    sh 'go test ./repository ./service ./controller -cover'
                }
            }
            stage('Deploy'){
                input{
                    message "Should we deploy?"
                    ok "Yes"
                    submitter "anotherDanger"
                    parameters{
                        string(name: 'INPUT_NAME', defaultValue: 'guest', description: 'Your name sir?')
                        choice(name: 'TARGET_ENV', choices: ['dev', 'stag', 'prod'], description: "Where?")
                    }
                }
                agent{
                    node{
                        label 'golang && almalinux'
                    }
                }

                when{
                    environment name: 'TARGET_ENV', value: 'prod'
                }

                steps{
                    echo("Deployed to ${TARGET_ENV} by ${INPUT_NAME}")
                }
            }
        }
    post{
        always{
            echo("Starting........")
        }

        success{
            echo("Completed, and success boss!")
        }

        failure{
            echo("Build failed")
        }

        cleanup{
            echo("End.")
        }
    }
}