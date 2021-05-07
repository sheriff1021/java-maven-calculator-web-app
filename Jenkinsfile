#!groovy
pipeline{
	agent any
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
	}
}
