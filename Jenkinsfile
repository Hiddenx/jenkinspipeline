pipeline {
    agent any

    tools {
    maven 'localMAven'
    }


    stages{
        stage('Build Package'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deploy to staging : local container localhost:8090'){
          steps{
            build job: 'deploy-to-staging'
          }
        }

        stage ('Deploy to Production : local container localhost:9090'){
          steps{
            timeout(time:5, unit:'DAYS'){
              input message: 'Approve Production Deployment?'
            }

            build job: 'deploy-to-prod'
          }

          post{
            success{
              echo 'Code Deployed to Production'
            }

            failure{
              echo 'Deployment failed'
              //send an email to developer/devOps engineer
            }
          }
        }
    }
}
