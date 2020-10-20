pipeline{
    agent {label 'windows'}
	
    stages {

		stage ('Clean') {
			steps {
				bat "dir"
				bat "dir MatchingGame"
                dir ('MatchingGame') {
					deleteDir()
				}
			}
		}
        
        stage('SCM'){
            steps {
		        checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/rolger/matching-game']]])
            }
	    }

        stage('Build') {
            steps {
                dir ('MatchingGame') {
					// def msbuild = '"C:\\Program Files (x86)\\Microsoft Visual Studio\\2019\\Professional\\MSBuild\\Current\\Bin\\MSBuild.exe\"'
                
					bat 'dotnet restore'
                   
                    bat '"C:\\Program Files (x86)\\Microsoft Visual Studio\\2019\\Professional\\MSBuild\\Current\\Bin\\MSBuild.exe" MatchingGame.sln /verbosity:minimal /nologo /t:Clean,Build /p:Configuration=Release /p:Platform=\"Any CPU\" /p:ProductVersion=1.0.0.${env.BUILD_NUMBER}"'
                }
            }
        }
        
        stage('Archive') {
            steps {
                dir ('MatchingGame/bin/Release') {
                    //zip zipFile: 'MatchingGame.zip'
					bat '"C:\\Program Files\\7-Zip\\7z.exe" a  -r MatchingGame.zip'
                }
				archiveArtifacts artifacts: '**'
            }
        }
    }
}