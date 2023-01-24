pipeline {
	    agent any
	
	        // Environment Variables

	
	    stages {

	        // Printing Basic Information
	        stage('Create File'){
	            steps {
	                
					script{
						checkout scm
						def fileContent = "{\n" +
							"	\"in_GitlabRepositoryLink\": \"Data1\",\n" +
							"	\"in_DesignerEmail\": \"\",\n" +
							"	\"in_BuildNumber\": \"Data2\"\n" +
							"}"
						def file = new File("TestFile.json")
						file.write(fileContent)
					}

	            }
	        }
	

	         // Build Stages
	        stage('Git') {
	            steps {
	                echo "Building..with ${WORKSPACE}"
	                
				}
	        }		
			
	    }
	
	    // 
	    post {
	        success {
	            echo 'Deployment has been completed!'
	        }
	        failure {
	          echo "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.JOB_DISPLAY_URL})"
	        }
	        always {
	            /* Clean workspace if success */
	            cleanWs()
	        }
	    }
	}
