pipeline {
    agent any
     tools {
        // Specify the Node.js installation by name
        nodejs 'nodejs'
      }
    stages {
         stage('Prepare') {
            steps {
                // Define the label for the Windows agent
                script {
                    agentLabel = ' LinuxNode-ProdServer'
                }
                sh 'pwd'
                sh "ls"
              //script {
              //cleanWs()
                   sh 'node --version'
              //sh 'npm cache clean --force'
              //sh 'npm install -g npm@latest'

					         sh 'npm install --legacy-peer-deps'    
              sh "npm run build" 
                //   }
                stash name: 'build-artifacts', includes: '**/*', excludes: 'workspace**'                
            }
        }
        stage('Package Install') {
            agent {                
                label agentLabel
            }
            options {
                skipDefaultCheckout true
            }
            steps {               
               
                 dir("workspace"){
                    unstash 'build-artifacts'
                    sh "ls"
                    sh 'pwd'
                   
                   sh 'sudo su'
                   script {
                   sh 'node --version'
                     //cleanWs()
                     //sh 'npm cache clean --force'
				           //sh 'npm install' 
                   //sh "npm run build"   
                   sh """
                        pwd
                        ls
                        docker images
                        docker build -t dsahoo165/angular_ui:${env.BUILD_NUMBER} .
                        docker images
                        """

                    sh """
                        docker ps                 
                        docker-compose down
                        
                        export IMAGE=dsahoo165/angular_ui
                        export TAG=${env.BUILD_NUMBER}
                        export PORT_TO_RUN=8082
                        docker-compose up -d
                        
                        docker ps
                        """    
                   }
                     
                   
                 }
            }
        }
    }
}
