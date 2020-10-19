pipeline {
	agent {
		docker {
			image 'mcr.microsoft.com/dotnet/framework/sdk:4.8'
			label 'windows'
		}
	}
	
	stages {
		stage('Build') {
			bat 'msbuild'
		}
	}
}