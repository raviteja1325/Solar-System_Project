pipeline{

    agent any
    
    tools{

        nodejs "NodeJS 26.2.0"
    }

    environment{
        MONGO_URI = "mongodb+srv:supercluster.d83jj.mongodb.net/superData"

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

                        junit allowEmptyResults: true, testResults: 'dependency-check-junit.xml'

                        publishHTML([allowMissing: true, alwaysLinkToLastBuild: true, icon: '', keepAll: true, reportDir: './', reportFiles: 'dependency-check-jenkins.html', reportName: 'Dependency Check HTML Report', reportTitles: '', useWrapperFileDirectly: true])
                    }
                }

            }
        }

        
        stage("Unit Tests"){
            steps{
                withCredentials([usernamePassword(credentialsId: 'mongo-db-credentials', passwordVariable: 'MONGO_PASSWORD', usernameVariable: 'MONGO_USERNAME')]) {
                    sh "npm test"
                }
                junit allowEmptyResults: true, testResults: 'test-result.xml'
            }
        }
        
    }
}