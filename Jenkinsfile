pipeline {
    agent any
    stages {
        stage ('Build Servlet Project') {
            steps {
               //sh  'mvn clean package'
                build job : 'Packer_servlet_project'

            }

            post{

                success{
                    echo 'Now Analyzing code ....'

                    //archiveArtifacts artifacts : '**/*.war'
                    build job : 'Static_AnalySis _Servlet_App'
                }
                failure{
                    echo "Error During Analyzing..."
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
