pipeline{
    options {
    ansiColor('xterm')
  }
    environment {
        imagename = "oussama24/frontendapp"
        registryCredential = "dockerhub_credentials"
        dockerImage = 'frontendapp'
    }
     agent {
    kubernetes {
      yamlFile 'builder.yaml'
    }
  }
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
        stage('Deploy App to Kubernetes') {     
      steps {
        container('kubectl') {
          withCredentials([file(credentialsId: 'mykubeconfig', variable: 'KUBECONFIG')]) {
            sh 'sed -i "s/<TAG>/${BUILD_NUMBER}/" front-deployment.yml'
            sh 'kubectl apply -f front-deployment.yml'
          }
        }
      }
    }
        }
    }
