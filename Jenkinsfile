pipeline{
    environment {
        imagename = "oussama24/frontendapp"
        registryCredential = "dockerhub_credentials"
        dockerImage = 'frontendapp'
    }
    agent any
    stages{

            
        
       //  stage('SonarQube analysis') {
                    
    //         steps{
    //             script {
    //            scannerHome = tool name: 'SonarScanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
    //                 withSonarQubeEnv('sonarqube-server') { 
        
    //                    sh "${scannerHome}/bin/sonar-scanner"
                     
    //                 }
    //             }         
    //         }
    //     }
        
        stage("build"){
            
            steps{
                sh 'npm install'
                sh 'npm run build'
            }
        }
      
        stage("docker-build"){
            steps{  
                    
                    script {
                    dockerImage = docker.build imagename   
                    docker.withRegistry( '', registryCredential ) {
                    dockerImage.push("$BUILD_NUMBER")
                    dockerImage.push('latest')
                    }
                }
            }
        }
         stage("deploy"){
             steps{ 
            script {
          kubernetesDeploy(configs: "frontend-deployment.yml", kubeconfigId: "kubernetes")
          
        }
      }
    }
        }
    }
