#!groovy
pipeline{
	agent any
	triggers {
		pollSCM('H/5 * * * *') 
		}
	environment{
		BLD=sh "echo $BUILD_NUMBER"
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
					sh "cd /var/lib/jenkins/workspace/job-1"
					sh "tar -czf calculator.tar.gz calculator.war"
					sh "curl -v --user 'admin:admin' --upload-file ./calculator.tar.gz http://localhost:8081/repository/maven-releases/com/company/sample-app/${BUILD_NUMBER}/calculator.tar.gz"					
				}
		}
		stage("make work downstream job"){				
				steps{
					build job: "job-2", parameters: [[$class: 'StringParameterValue', name: 'numba', value: env.BLD]], wait: true
				}
		}

	}
}
