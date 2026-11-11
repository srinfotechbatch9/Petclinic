pipeline{
    agent any
    stages{
        
        stage('Clone'){
            
            steps{
                
              git branch: 'feature/2026.11.11', url: 'https://github.com/srinfotechbatch9/Petclinic.git'
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

               'junit 'target/surefire-reports/*.xml'
            }
        }
        stage('Published Artifcats'){

            steps{

                archiveArtifacts artifacts: 'target/*.war', followSymlinks: false
            }
        }
        stage('Deploy'){

            steps{

              deploy adapters: [tomcat9(alternativeDeploymentContext: '', credentialsId: 'Tomcatcredentials', path: '', url: 'http://localhost:8080/')], contextPath: 'Petclinic', war: 'target/*.war'
            }
        }
    }
}