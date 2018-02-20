pipeline{
  agent any

  parameters {
        string(name: 'tomcat_dev',defaultValue: '35.154.153.67', description: 'Staging server')
        string(name: 'tomcat_prod',defaultValue: '13.126.146.72', description: 'Production server')
      }

  triggers{
    pollSCM('* * * * *')
  }

  stages{
    stage('Build Package'){
      steps{
              sh 'mvn clean package'
      }

      post {
        success {
          archiveArtifacts artifacts: '**/target/*.war'
        }
      }

    }

  stage('Deploy to staging'){

    steps{
          sh "scp -i ~/Desktop/ssh/EC2-DEMO.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
    }

  }

  stage('Deploy to Production'){

    steps{
      timeout(time:5, unit:'DAYS'){
        input message: 'Approve Production Deployment?'
      }
          sh "scp -i ~/Desktop/ssh/EC2-DEMO.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
    }

  }


}
