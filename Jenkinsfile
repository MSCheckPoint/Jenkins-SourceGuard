 pipeline {
      agent any
      environment {
           SG_CLIENT_ID = credentials("martyre37")
           SG_SECRET_KEY = credentials("4090ff48-8776-4873-8aa7-244e8cf790e8")
           registry = "https://registry.hub.docker.com"
       
           dockerImage = 'infoslack/dvwa'
        }
  stages {
          
         stage('Clone Github repository') {
            
    
           steps {
              
             checkout scm
           
             }
  
          }
    stage('SourceGuard Code Scan') {   
       steps {   
                   
         script {      
              try {
         
               
            
                sh 'chmod +x sourceguard-cli' 

                sh './sourceguard-cli --src .'
           
               } catch (Exception e) {
    
                 echo "Request for Approval"  
                  }
              }
            }
         }
           
           
          stage('Docker image Build and scan prep') {
             
            steps {

              sh 'docker build -t infoslack/dvwa .'
              sh 'docker save infoslack/dvwa -o sg.tar'
              
             } 
           }
       stage('SourceGuard Container Image Scan') {   
          steps {
            script {
               try {     
           
         
                    sh './sourceguard-cli --img sg.tar'
                    } catch (Exception e) {
    
                 echo "Image scanning is BLOCK and recommend not using the source code"  
                  }
                }
              }
            
            }
            
        
        
  } 
}
