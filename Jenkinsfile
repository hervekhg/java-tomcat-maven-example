pipeline {
    agent any
    stages {
        stage ('Build Servlet Project') {
            steps {
                /*For Mac & Linux Machine */
               sh  'mvn clean package'

            }

            post{

                success{
                    echo 'Now Archiving ....'

                    archiveArtifacts artifacts : '**/*.war'
                }
                failure{
                    echo "Error During Archiving..."
                }
            }
        }

        stage ('Deploy Build in Staging Area'){
            steps{

                build job : 'Deploy_Serverlet_Staging_Env'

            }
        }

        stage ('Deploy to Production'){
            steps{
                timeout (time: 5, unit:'DAYS'){
                    input message: 'Approve PRODUCTION Deployment?'
                }
                
                build job : 'Deploy_To_Production'
            }

            post{
                success{
                    echo 'Deployment on PRODUCTION is Successful'
                }

                failure{
                    echo 'Deployement Failure on PRODUCTION'
                }
            }
        }
    }
}
