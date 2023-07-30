currentBuild.description = "branchName: ${env.branchName}"
pipeline {
	agent { label 'build01'}
	options { skipDefaultCheckout true }

	stages {
		stage("CheckOut"){
			steps {
				script{
					checkout([$class: 'GitSCM', 
							   branches: [[name: "${env.branchName}"]], 
							   extensions: [], 
							   userRemoteConfigs: [[
				   			   		credentialsId: '9d02cccf-5f45-46d6-b4e5-81ff97809c12', 
				   			   		url: "${env.srcUrl}"]]])

				}
			}
		}

		stage("Build"){
			steps {
				script{
					switch("${env.buildTools}") {
						case "npm":
						   sh "npm install && npm run build"
						   break
						case "yarn":
						   sh "yarn build"
						   break
						default:
						   echo "error"
						   break						   							
					}
					if ("${JOB_NAME}".endsWith("-web")) {
						env.skipUnitTest = 'true'					
					} else {
						env.skipUnitTest = 'false'						
					}
				}
			}

		}

		stage("Test"){
			when {
			  environment name: 'skipUnitTest', value: 'false'
			}			
			steps{
				script{
					sh "gradle test"

				}
			}
		}

	}
}
