pipeline {
	    agent any
	

	        // Environment Variables
	        environment {
	        MAJOR = '1'
	        MINOR = '0'
	        //Orchestrator Services
	        UIPATH_ORCH_URL = "https://cloud.uipath.com/"
	        UIPATH_ORCH_LOGICAL_NAME = "testkyqlzlg"
	        UIPATH_ORCH_TENANT_NAME = "AUX"
	        UIPATH_ORCH_FOLDER_NAME = "TEST"
			UIPATH_ORCH_CREDENTIALS = "Orchestrator"
		
			
	    }
	

	    stages {
	

	        // Printing Basic Information
	        stage('Preparing'){
	            steps {
	                echo "Jenkins Home ${env.JENKINS_HOME}"
	                echo "Jenkins URL ${env.JENKINS_URL}"
	                echo "Jenkins JOB Number ${env.BUILD_NUMBER}"
	                echo "Jenkins JOB Name ${env.JOB_NAME}"
	                echo "GitHub BranhName ${env.BRANCH_NAME}"
	                checkout scm
	

	            }
	        }
	

	         // Build Stages
	        stage('Build') {
	            steps {
	                echo "Building..with ${WORKSPACE}"
	                UiPathPack (
	                      outputPath: "Output\\${env.BUILD_NUMBER}",
	                      projectJsonPath: "project.json",
	                      version: [$class: 'ManualVersionEntry', version: "${MAJOR}.${MINOR}.${env.BUILD_NUMBER}"],
	                      useOrchestrator: false,
						  traceLevel: 'None'
	        )
	            }
	        }
			
			 // Deploy stages IMPLEMENTING
	        stage('Deploy to Stage') {
	            steps {
	                echo "Building..with ${WORKSPACE}"
					UiPathDeploy (
							orchestratorAddress: "${UIPATH_ORCH_URL}",
							orchestratorTenant: "${UIPATH_ORCH_TENANT_NAME}",
							folderName: "${UIPATH_ORCH_FOLDER_NAME}",
							credentials: Token(accountName: "${UIPATH_ORCH_LOGICAL_NAME}", credentialsId: "${UIPATH_ORCH_CREDENTIALS}"), // Automation Cloud
							//credentials: [$class: 'UserPassAuthenticationEntry', credentialsId: "${UIPATH_ORCH_STAGE_CREDENTIALS}"], // On-Premise
							packagePath: "${UIPATH_PACKAGE}",
							traceLevel: 'None',
							environments: ''//,
							entryPointPaths: "Main.xaml"
					)


	            }
	        }
			
			
			
	         // Test Stages
	        stage('Test') {
	            steps {
	                echo 'Testing..the workflow...'
	            }
	        }
	


	

	         // Deploy to Production Step
	        stage('Deploy to Production') {
	            steps {
	                echo 'Deploy to Production'
	                }
	            }
	    }
	

	    // Options
	    options {
	        // Timeout for pipeline
	        timeout(time:80, unit:'MINUTES')
	        skipDefaultCheckout()
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
