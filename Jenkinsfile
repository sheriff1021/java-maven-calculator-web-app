#!groovy
pipeline{
	agent any
	triggers {
		pollSCM('H/5 * * * *') 
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
					sh "tar -czf calculator.war calculator.tar.gz"
					sh "curl -v --user 'admin:admin' --upload-file ./calculator.tar.gz http://localhost:8081/repository/maven-releases/com/company/sample-app/${BUILD-NUMBER}/calculator.tar.gz"					
				}
		}

	}
}
