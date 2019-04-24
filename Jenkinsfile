def appname = 'My Jenkins Build: '
def jenkinsUrl = "${env.JENKINS_URL}"
def JOB_NAME = "${env.JOB_NAME}"
def BUILD_ID = "${env.BUILD_ID}"
def mailNotifier = 'jenkins@' + jenkinsUrl.replace('https://', '').replace('.com/','com')
String cron_string = "H/30 * * * *"

                                                                           
pipeline {
    agent any

    //Found that parameters tag only working for python code for ansible you need to pass parameter using job
    /*parameters {
    string(name: 'BUCKET', defaultValue: 'TEST231nhy', description: '')
    string(name: 'REGION', defaultValue: 'ap-south-1', description: '')
    }*/
   
  //To apply cron trigger
  /*triggers {
        cron(cron_string)
        }*/


    stages {
       /*stage('s3 bucket create using python') {
       //Note: if want to run pipeline code with role based login
       //withAWS(role:"$ROLE_NAME", roleAccount: "$MASTER_ACCOUNT"){
                echo 'S3 Bucket creation using python'
                sh 'python create_s3_bucket.py '
                }
           }*/
        stage('s3 Bucket creation using ansible') {
            steps {
                echo 'S3 Bucket ${params.BUCKET} creation using ansible'
               // sh 'ansible-playbook s3.yml -e "myBucketName=$BUCKET  myRegion=$REGION" ' //use it if want to override variable
                sh 'ansible-playbook s3.yml '
            }
        /*stage('s3 Bucket creation using Terraform') {
            steps {
                echo 'S3 Bucket ${params.BUCKET} creation using terraform'
                // Needed to delete terraform.tfstate files in workspace before doing next build otherwise it automaticaly destroy the things created on previous build 
                // Note: Its not working perfectly please research on it
                 //sh ' terraform state rm  module.aws_s3_bucket.example.aws_s3_bucket ' 
                 sh ' terraform init terraform ' //only needed first time when you run project
                 sh ' terraform apply -auto-approve -var-file="terraform/custom.tfvars" terraform '
            }
        }*/
    }
  
    /*  post {
            success {
                echo "Test run completed succesfully."
            }
            failure {
                echo "Test run failed."
            }
            always {
        // Let's wipe out the workspace before we finish!
                deleteDir()
                echo "Workspace cleaned"
            }
      }*/
    
  post {  
    always {
      mail (body: "This is the Build ID: ${BUILD_ID} \n Thanks& Regards #cp  ",
            from: "${mailNotifier}",
            replyTo: 'donotreply@gmail.com',
            subject: "${appname} ${JOB_NAME}",
            to: 'chandra.prakash363@gmail.com')

   }
 }
        
     // The options directive is for configuration that applies to the whole job.
    options {
        // For example, we'd like to make sure we only keep 10 builds at a time, so
        // we don't fill up our storage!
        buildDiscarder(logRotator(numToKeepStr:'10'))

        // And we'd really like to be sure that this build doesn't hang forever, so
        // let's time it out after an hour.
        timeout(time: 60, unit: 'MINUTES')
}
} 
