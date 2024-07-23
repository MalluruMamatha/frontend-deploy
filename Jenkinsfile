pipeline{
    agent {
        label 'agent-1'
    }
    options{ /// using option we can specify the time that pipeline should execute 
    //(with in the time it get complets)
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds()
        ansiColor('xterm')
       
    }

    parameters {
        string(name: 'appversion', defaultValue: '1.0.0', description: 'what is the version?')
    }

    environment{

        def appversion = '' // variable declaration
        def nexusUrl = 'nexus.malluru.online:8081'
     
    }

   stages{

        stage('print the version'){
            steps{
                script{
                    echo "application version is: ${params.appversion}"
                }
            }
        }
        stage('init'){
            steps{
                sh """
                    cd terraform
                    terraform init -reconfigure
                """
            }
        }

        stage('plan'){
            steps{
                sh """
                    pwd
                    cd terraform
                    terraform plan -var="app_version=${params.appversion}"
                """
            }
        }

         stage('Deploy'){
            steps{
                sh """
                    cd terraform
                    terraform apply -auto-approve -var="app_version=${params.appversion}"
                """
            }
        }
    }






    post{

        always{
            echo 'always say hello' 
           deleteDir()   ////delete workspace when build is done
        }
        success{
            echo 'i will run when pipeline is success'

        }
        failure{
            echo 'i will run when pipeline is failure'

        }
    }
}



