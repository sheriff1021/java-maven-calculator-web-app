#!groovy
pipeline{
	agent any
	triggers {
		pollSCM('H/5 * * * *') 
		}
	environment{
		custom_var=sh (script: '''echo $BUILD_NUMBER''', returnStdout: true).trim()

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
		stage("make work downstream job-1"){				
				steps{					

					build job: "job-2", parameters: wait: true
				}
		}

	}
}
