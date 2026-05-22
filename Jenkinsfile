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
  
        stage("NPM Dependency AUdit"){
            steps{
                
                sh'''
                    npm audit --audit-level=critical
                    echo $?
                '''
            }
        }

        stage("OWASP Dependency Check"){
            steps{
                
                dependencyCheck additionalArguments: '''
                    --scan \'./\'
                    --out \'./\'
                    --format \'ALL\'
                    --prettyPrint''',odcInstallation: 'OWASP-DP-CHECK-12' 
            }
        }
    }
}