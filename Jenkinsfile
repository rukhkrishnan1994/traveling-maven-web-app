node{
    
echo" the job name  is : $JOB_NAME"
echo" the name  bil tag is : $BUILD_TAG"
echo" the work space is: $WORKSPACE"

def mavenHome = tool name: "maven3.9.2"
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM(ignorePostCommitHooks: true, scmpoll_spec: '* * * * *')])])
stage("checkout"){


git credentialsId: 'a8cb085c-3d8b-4624-8eb0-9c58858e2213', url: 'https://github.com/Time-traveling/travalling-maven-web-app.git'

}

stage("build"){

sh "${mavenHome}/bin/mvn clean package"
}  // maven build end

stage("upload sonar"){

sh "${mavenHome}/bin/mvn clean sonar:sonar"
} // sonar end 

stage("upload nexus"){

sh "${mavenHome}/bin/mvn clean deploy"
} // nexus end

stage("upload tomcate"){
sshagent(['6e795211-063a-43a1-949d-b89f81204d0d']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.33.107:/opt/apache-tomcat-9.0.75/webapps/"	
} //ssh block end

} // stage tomcate end

} // stage end 
