def appname = 'My Jenkins Build: '
def jenkinsUrl = "${env.JENKINS_URL}"
def JOB_NAME = "${env.JOB_NAME}"
def BUILD_ID = "${env.BUILD_ID}"
def mailNotifier = 'jenkins@' + jenkinsUrl.replace('https://', '').replace('.com/','com')
String cron_string = "H/30 * * * *"

                                                                           
pipeline {
    agent any
    stages {
      stage('s3 Bucket creation using ansible') {
            steps {
                echo 'S3 Bucket ${params.BUCKET} creation using ansible'
               // sh 'ansible-playbook s3.yml -e "myBucketName=$BUCKET  myRegion=$REGION" ' //use it if want to override variable
                sh 'ansible-playbook s3.yml '
            }
       }
    }
  
    
    
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
