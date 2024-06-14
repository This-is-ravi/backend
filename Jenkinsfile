pipeline {
    agent{
        label 'AGENT-1'
    }
    options {
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds() 
        ansiColor('xterm')
    }
    environment{
        def appVersion = '' //Global variable declaration
        
    }
    
   
    stages {
        stage('read the version'){
            steps{
                script{  //its a Groovy script
                    def packageJson = readJSON file: 'package.json'
                    appVersion = packageJson.version
                    echo "application version: $appVersion"
                }
                
            }

        }
        stage('Install Dependencies') {
            steps {
                sh """
                npm install
                ls -ltr
                echo "application version: $appVersion"
                """
            }
        }

        stage('Build'){
            steps{ // excluding the backend.zip 
                sh """
                zip -q -r backend-${appVersion}.zip * -x Jenkinsfile -x backend-${appVersion}.zip 
                ls -ltr 
                """ // ls -ltr to know zip file created or not
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

}