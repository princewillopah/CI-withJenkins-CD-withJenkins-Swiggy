pipeline{
     agent any
     
     tools{
         jdk 'jdk17'
         nodejs 'my-nodejs'
     }
     environment {
         SCANNER_HOME=tool 'my-sonarqube-scanner'
     }
     
     stages {
         stage('Clean Workspace'){
             steps{
                 cleanWs()
             }
         }
         stage('Checkout from Git'){
             steps{
                 git branch: 'master', url: 'https://github.com/princewillopah/CI-withJenkins-CD-withJenkins-Swiggy.git'
             }
         }
         stage("Sonarqube Analysis "){
             steps{
                 withSonarQubeEnv('My-SonarQube-servers-1') {
                     sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=CI-withJenkins-CD-withJenkins-Swiggy \
                     -Dsonar.projectKey=CI-withJenkins-CD-withJenkins-Swiggy '''
                 }
             }
         }
         stage("Quality Gate"){
            steps {
                 script {
                     waitForQualityGate abortPipeline: false, credentialsId: 'sonarqube-token' 
                 }
             } 
         }
         stage('Install Dependencies') {
             steps {
                 sh "npm install"
             }
         }
         stage('TRIVY FS SCAN') {
             steps {
                 sh "trivy fs . > trivyfs.txt"
             }
         }
        //   stage("Docker Build & Push"){
        //      steps{
        //          script{
        //             withDockerRegistry(credentialsId: 'dockerhub', toolName: 'docker'){   
        //                 sh "docker build -t swiggy-clone ."
        //                 sh "docker tag swiggy-clone ashfaque9x/swiggy-clone:latest "
        //                 sh "docker push ashfaque9x/swiggy-clone:latest "
        //              }
        //          }
        //      }
        //  }
        //  stage("TRIVY"){
        //      steps{
        //          sh "trivy image ashfaque9x/swiggy-clone:latest > trivyimage.txt" 
        //      }
        //  }
        //   stage('Deploy to Kubernets'){
        //      steps{
        //          script{
        //              dir('Kubernetes') {
        //                  kubeconfig(credentialsId: 'kubernetes', serverUrl: '') {
        //                  sh 'kubectl delete --all pods'
        //                  sh 'kubectl apply -f deployment.yml'
        //                  sh 'kubectl apply -f service.yml'
        //                  }   
        //              }
        //          }
        //      }
        //  }



     }
 }