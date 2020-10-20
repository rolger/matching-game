pipeline{
    agent {label 'windows'}
    
	
    stages {
        
        stage('SCM'){
            steps {
		        checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/rolger/matching-game']]])
            }
	    }
		
	
		stage ('Clean') {
			steps {
                dir ('MatchingGame') {
					deleteDir()
				}
				bat "dotnet clean"
			}
		}
	
        stage('Build') {
            steps {
                dir ('MatchingGame') {
					def msbuild = "\"C:\\Program Files (x86)\\Microsoft Visual Studio\\2019\\Professional\\MSBuild\\Current\\Bin\\MSBuild.exe\""
                
					bat 'dotnet restore'
					bat 'nuget restore BigProject.sln'
                   
                    bat '${msbuild} MatchingGame.sln /verbosity:minimal /nologo /t:Clean,Build /p:Configuration=Release /p:Platform=\"Any CPU\" /p:ProductVersion=1.0.0.${env.BUILD_NUMBER}"'
                }
            }
        }
        
        stage('Archive') {
            agent {label 'master'}
            steps {
                dir ('MatchingGame/bin/Release') {
                    zip zipFile: 'MatchingGame.zip'
					
                }
				archiveArtifacts artifacts: '**'
            }
        }
    }
}