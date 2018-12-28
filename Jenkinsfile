pipeline {
    agent any
    stages{
        stage('Build'){
            steps {
                bat 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage ('Deploy to Staging'){
            steps {
                build job: 'deployment-job-staging'
            }
        }
        stage ('Deploy to Production'){
            steps{
                timeout(time:5, unit:'DAYS'){
                input message: 'Approval for Deployment to Production'
                }
                build job: 'deployment-job-production'
            }
            post{
                success{
                    echo 'Code Deployed to Production as well'
                }
                failure{
                    echo 'Deployment failed'
                }
            }
        }
    }
}