pipeline {
    agent any
    
    environment{
        registry = "374720440341.dkr.ecr.us-east-1.amazonaws.com/jedo_repo"
    }
    stages{
        
        
        stage ('Checkout') {
            steps{
            checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/snehal1909/Docker_ecr_jenkins.git']]])
            
            }
        }
        
    
        stage ('Docker Build'){
            steps{
                   script {
                       dockerImage = docker.build registry
                   }
                
            }
        }
        stage ('Docker Push'){
            steps{
                 script{
                    sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 374720440341.dkr.ecr.us-east-1.amazonaws.com'
                    sh 'docker push 374720440341.dkr.ecr.us-east-1.amazonaws.com/jedo_repo:latest' 
                 }
                
            }
        }
        
        
        
		  // Stopping Docker containers for cleaner Docker run
        Stage ('stop previous containers'){
            steps {
                sh 'docker ps -f name=mypythonContainer -q | xargs --no-run-if-empty docker container stop'
                sh 'docker container ls -a -fname=mypythonContainer -q | xargs -r docker container rm'
            }
        }
        
        stage('Docker Run') {
     steps{
         script {
                sh 'docker run -d -p 8096:5000 --rm --name mypythonContainer 374720440341.dkr.ecr.us-east-1.amazonaws.com/jedo_repo:latest'
    }
     }
        }
    }
}
