node {
	stage('SCM'){
		checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/rolger/matching-game']]])
	}
	
	stage('Build'){
		docker.image('mcr.microsoft.com/dotnet/framework/sdk:4.8').inside {
		
		
		}
	}
}