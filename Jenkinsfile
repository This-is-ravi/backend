pipeline {
    agent{
        label 'AGENT-1'
    }
    options {
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds() 
        ansiColor('xterm')
    }
    parameters {  // we can destroy or apply the pipeline
        choice(name: 'action', choices: ['Apply', 'Destroy'], description: 'Pick something')
    }
   
    stages {
        stage('init') {
            steps {
                sh """
                cd 01-vpc
                terraform init -reconfigure

                """
            }
        }
        stage('plan') {
            when {
                expression{
                    params.action == 'Apply'
                }
            }
            steps {
                sh """
                cd 01-vpc
                terraform plan
                """
            }
        }
        stage('Deploy') {
            when {
                expression{
                    params.action == 'Apply'
                }
            }
            input{
                message "should we continue ?"
                ok "Yes, we should."
            }

            steps {
                sh """
                cd 01-vpc
                terraform apply -auto-approve
                """
            }
        }

        stage('Destroy') {
            when {
                expression{
                    params.action == 'Destroy'
                }
            }
            steps {
                sh """
                cd 01-vpc
                terraform destroy -auto-approve
                """
            }
        }
        
    }
    post { 
        always {  // delete the workspace build after new build starts
            echo 'I will always say Hello again!'  
            deleteDir()
        }
        success { 
            echo 'I will run when pipeline is success'
        }
        failure { 
            echo 'I will run when pipeline is failure'
        }
    }
}