pipeline{
    agent {label 'windows'}
	
    stages {

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
			
                dir ('MatchingGame/MatchingGame/bin/Release') {
					script {
						def commitCount = bat(script: 'git rev-list --count HEAD', returnStdout: true).split('\\r?\\n')[2]
						println "git commit count $commitCount"

						def newVersion = "1.0." + commitCount
						println "New computed version $newVersion"

						input 'cont'

						println '"C:\\Program Files\\7-Zip\\7z.exe" a  -r MatchingGame_v$newVersion.zip'
						println "MatchingGame_v$newVersion"
						
						
						println "C:\\Program Files\\7-Zip\\7z.exe a  -r MatchingGame_v$newVersion"
			
						input 'cont'
						// println "\"C:\\Program Files\\7-Zip\\7z.exe\" a  -r MatchingGame_v$newVersion.zip"
					}
                }
				// archiveArtifacts artifacts: 'MatchingGame/MatchingGame/bin/Release/MatchingGame*.zip'
            }
        }
    }
}