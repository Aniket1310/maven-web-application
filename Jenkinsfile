node{
    echo "the job name is ${env.JOB_NAME}"
    echo "the build number is ${env.BUILD_NUMBER}"
    echo "The node name is ${env.NODE_NAME}"
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])

def mavenHome = tool name: 'maven 3.9.1'

stage('checkoutcode'){
        git credentialsId: '0f67a85a-57f8-4e25-849f-d985b1f38b97', url: 'https://github.com/Aniket1310/maven-web-application.git'
    }
    
stage('Build'){
        sh "${mavenHome}/bin/mvn clean package"
    }
    
stage('sonar'){
    sh "${mavenHome}/bin/mvn clean sonar:sonar"
}

stage('nexus'){
    sh "${mavenHome}/bin/mvn clean deploy"
}

stage('tomcat'){
sshagent(['65b6be55-e824-4635-83c9-891849ba840e']) {
   sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.36.52:/opt/tomcat/webapps/"
}  
}

}
