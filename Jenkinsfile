pipeline{

    agent any
    
    tools{

        nodejs "NodeJS 26.2.0"
    }

    stages{
        stage("Installing Dependencies"){
            steps{
                
                sh "npm install --no-audit"
            }
        }
    }    
   
}