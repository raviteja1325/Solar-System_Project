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
        
        stage("Dependency Scanning"){
            parallel{
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
                        dependencyCheckPublisher failedTotalCritical:1, pattern: 'dependency-check-report.xml', stopbuild: true
                        publishHTML([allowMissing: true, alwaysLinkToLastBuild: true, icon: '', keepAll: true, reportDir: './', reportFiles: 'dependency-check-jenkins.html', reportName: 'Dependency Check HTML Report', reportTitles: '', useWrapperFileDirectly: true])
                    }
                }

            }
        }

        
    }
}