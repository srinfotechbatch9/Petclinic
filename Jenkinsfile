pipeline{
    agent any
    stages{
        
        stage('Clone'){
            
            steps{
                
              git branch: 'main', url: 'https://github.com/srinfotechbatch9/Petclinic.git'
            }
        }
        stage('Build'){
            
            steps{
                bat 'mvn install'
            }
        }
        stage('Test'){

            steps{

                bat 'mvn test'
            }
        }
		 stage('Generated test Results'){

            steps{

              junit 'target/surefire-reports/*.xml'
            }
        }
        
         stage('Sonarqube Analysis'){

            steps{

             bat '''mvn install verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar \
             -Dsonar.projectKey=spring-petclinic \
             -Dsonar.projectName='spring-petclinic' \
             -Dsonar.host.url=http://localhost:9000 \
             -Dsonar.token=sqp_002f00ab29402d39aada84f68a8cd52ae20ab536'''
            }
        }
        stage('Published Artifcats'){

            steps{

                archiveArtifacts artifacts: 'target/*.war', followSymlinks: false
            }
        }
        
        stage ('Artifactory Server'){

    steps {
    
       rtServer (
       
         id: "Artifactory",
         
         url: 'http://localhost:8081/artifactory',
         
         username: 'admin',
         
          password: 'password',
          
          bypassProxy: true,
          
           timeout: 300
           
                )
    }
    
}
stage('Upload'){

    steps{
    
        rtUpload (
        
         serverId:"Artifactory" ,
         
          spec: '''{
          
           "files": [
           
              {
              
              "pattern": "*.war",
              
              "target": "srinfotech-batch9"
              
              }
              
                    ]
                    
                   }''',
                   
                )
    }
    
}
stage ('Publish build info') {

    steps {
    
        rtPublishBuildInfo (
        
            serverId: "Artifactory"
        )
        
    }
    
}
        
        stage('Deploy'){

            steps{

              deploy adapters: [tomcat9(alternativeDeploymentContext: '', credentialsId: 'Tomcatcredentials', path: '', url: 'http://localhost:8080/')], contextPath: 'Petclinic', war: 'target/*.war'
            }
        }
    }
}
