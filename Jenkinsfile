#!groovy
pipeline{
	agent any
	triggers {
		pollSCM('H/5 * * * *') 
		}
	environment{
		custom_var=sh (script: '''echo $BUILD_NUMBER''', returnStdout: true).trim()
		JOB_NAME=(script: '''echo job-1''', returnStdout: true).trim()

	}
	stages{
		stage("checkout"){
				steps{
					git([url: 'https://github.com/sheriff1021/java-maven-calculator-web-app.git',branch: 'vova'])	
				}
		}
		stage("testing"){
				steps{
					sh "mvn clean test"
				}
		}
		stage("unit-test"){
				steps{
					sh "mvn integration-test"
				}
		}
		stage("performance-test"){
				steps{
					sh "mvn verify"
				}
		}
		stage("war"){
				steps{
					sh "mvn package"
				}
		}
		stage("nexus"){
				steps{
					sh "cd /var/lib/jenkins/workspace/job-1/target"
					sh "tar -cf target/calculator.tar target/calculator.war"
					sh "curl -v --user 'admin:admin' --upload-file target/calculator.tar http://localhost:8081/repository/maven-releases/com/company/sample-app/${BUILD_NUMBER}/calculator.tar"					
				}
		}
		stage("make work downstream job-1"){				
				steps{					

					build job: "job-2", parameters: [[$class: 'StringParameterValue', name: 'numba', value: env.custom_var]], wait: true
				}
		}
		stage("make work downstream jab-2"){				
				steps{					

					build job: "jab-2", wait: true
				}
		}

	}
	post {
       		 always {
           	 	echo 'This will always run'
        	}
        	success {
            		mail bcc: '', body: "<b>Example</b><br>\n\<br>Project: ${env.JOB_NAME} <br>Build Number: ${env.custom_var} <br> URL de build: ${env.BUILD_URL}", cc: '', charset: 'UTF-8', from: '', 				mimeType: 'text/html', replyTo: '', subject: "ERROR CI: Project name -> ${env.JOB_NAME}", to: "vladimir_yarygin@epam.com";
       		}
        	failure {
           		 mail bcc: '', body: "<b>Example</b><br>\n\<br>Project: ${env.JOB_NAME} <br>Build Number: ${env.custom_var} <br> URL de build: ${env.BUILD_URL}", cc: '', charset: 'UTF-8', from: '', 				mimeType: 'text/html', replyTo: '', subject: "ERROR CI: Project name -> ${env.JOB_NAME}", to: "vladimir_yarygin@epam.com";
     	        }
        	unstable {
            		echo 'This will run only if the run was marked as unstable'
       		}
       	        changed {
            			echo 'This will run only if the state of the Pipeline has changed'
           		        echo 'For example, if the Pipeline was previously failing but is now successful'
        	}
    }
}
