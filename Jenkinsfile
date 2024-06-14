pipeline {
    agent{
        label 'AGENT-1'
    }
    options {
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds() 
        ansiColor('xterm')
    }
    
   
    stages {
        stage('test') {
            steps {
                sh """
                echo "this is testing"

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