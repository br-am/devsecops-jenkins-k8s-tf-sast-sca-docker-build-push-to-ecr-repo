pipeline {
  agent any
  tools { 
        maven 'manven-3.5.2'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=asecuritygroupkey -Dsonar.organization=asecuritygroupkey -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=7a8755db3115d05c0c082731c262b45f6f7c5e89'
			}
    }

	stage('RunSCAAnalysisUsingSnyk') {
            steps {		
				withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
					sh 'mvn install -Dsnyk.skip'
				}
			}
    }

	stage('Build') { 
            steps { 
               withDockerRegistry([credentialsId: "dockerlogin", url: ""]) {
                 script{
                 app =  docker.build("asg")
                 }
               }
            }
    }

	stage('Push') {
            steps {
                script{
                    docker.withRegistry('https://309520210477.dkr.ecr.us-east-2.amazonaws.com/bram', 'ecr:us-east-2:aws-credentials') {
                    app.push("latest")
                    }
                }
            }
    	}
	    
  }
}
