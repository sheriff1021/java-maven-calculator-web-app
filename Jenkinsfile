#!groovy
pipeline{
	agent any
	triggers {
		pollSCM('H/5 * * * *') 
		}
	stages{
		stage("checkout"){
				steps{
					git([url: 'https://github.com/sheriff1021/jjava-maven-calculator-web-app.git	calculator-web-app.git',branch: 'vova'])	
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
					sh "curl -v --user 'admin:admin' --upload-file ./target/calculator.war http://localhost:8081/repository/maven-releases/calculator.war"
				}
		}

	}
}
