pipeline {
    
    agent {
        label "linuxbuildnode"
    }
    
    
    stages{
        stage('SCM') {
            steps{
                echo "scm step"
                git  "https://github.com/Thesense3/jenkins-docker-maven-java-webapp.git"
            }
            
            
        }
        
        
        
         stage('build by maven'){
             steps{
                 echo "this is maven stage"
                 sh   "mvn clean package"
             }
             
        }
        
        stage('docker'){
            steps{
              sh "sudo docker build  -t  thesense03/javaweb:newversion4."
            }
        }    
        
        stage('docker registry'){
            steps{
                withCredentials([string(credentialsId: 'new_docker_hub_pw', variable: 'new_password_for_my_docker')]) {
                    sh "sudo docker login -u thesense03 -p $new_password_for_my_docker"
}
             
               
                sh "sudo docker push thesense03/javaweb:newversion4"
           }
        }
        
        stage('deploy'){
            steps{
                
            sh 'sudo docker rm -f javapp'   
             sh "sudo docker run -d -p 8080:8080 --name javapp thesense03/javaweb:newversion4"   
            }
        }
        stage('QA TEAM'){
            steps{
                sshagent(['QA_SSH']) {
        sh "ssh -o StrictHostKeyChecking=no ec2-user@43.205.230.80 sudo docker rm -f javapp"           
        sh "ssh ec2-user@43.205.230.80 sudo docker run -d -p 8080:8080 --name javapp thesense03/javaweb:newversion4"
}
            }
        }
        
        //stage('QA TEAM Testing'){
          //  steps{
            //    sh 'curl --silent http://43.205.230.80:8080/java-web-app/ | grep India'
        //    }
        //}
          
            stage('aproval') {
            steps{
               script {
                Boolean userInput = input(id: 'Proceed1', message: 'Promote build?', parameters: [[$class: 'BooleanParameterDefinition', defaultValue: true, description: '', name: 'Please confirm you agree with this']])
                echo 'userInput: ' + userInput

                if(userInput == true) {
                    // do action
                } else {
                    // not do action
                    echo "Action was aborted."
                }
               
            }
            
            } 
        }
            stage('PROD ENV'){
            steps{
                sshagent(['QA_SSH']) {
            sh "ssh -o StrictHostKeyChecking=no ec2-user@3.110.132.146 sudo docker rm -f javapp"           
            sh "ssh ec2-user@3.110.132.146 sudo docker run -d -p 8080:8080 --name javapp thesense03/javaweb:newversion4"
    }
              }
            }   
            
            
        
    
    
    
    
    
    
    

}
}
