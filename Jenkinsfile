pipeline{
    agent {label 'windows'}
    
    stages {
        
        stage('SCM'){
            steps {
		        checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/rolger/matching-game']]])
            }
	    }
	
        stage('Build') {
            steps {
                dir ('MatchingGame') {
                
                    echo 'Hello world!' 
                    bat '"C:\\Program Files (x86)\\Microsoft Visual Studio\\2019\\Professional\\MSBuild\\Current\\Bin\\MSBuild.exe" MatchingGame.sln /p:Configuration=Release /p:Platform=\"Any CPU\" /p:ProductVersion=1.0.0.${env.BUILD_NUMBER}"'
                }
            }
        }
        
        stage('Archive') {
            agent {label 'master'}
            steps {
                dir ('MatchingGame/bin/Release') {
                    zip zipFile: 'MatchingGame.zip'
                }
            }
        }
    }
}