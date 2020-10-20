pipeline{
    agent {label 'windows'}
	
    stages {

        stage('Build') {
            steps {
                dir ('MatchingGame') {
					bat 'dotnet restore'
                    bat '"C:\\Program Files (x86)\\Microsoft Visual Studio\\2019\\Professional\\MSBuild\\Current\\Bin\\MSBuild.exe" MatchingGame.sln /verbosity:minimal /nologo /t:Clean,Build /p:Configuration=Release /p:Platform=\"Any CPU\" /p:ProductVersion=1.0.0.${env.BUILD_NUMBER}"'
                }
            }
        }
        
        stage('Archive') {
            steps {
				script {
					dir ('MatchingGame/MatchingGame/bin/Release') {
						def commitCount = bat(script: 'git rev-list --count HEAD', returnStdout: true).split('\\r?\\n')[2]
						def newVersion = "v1.0." + commitCount + ".zip"
						println "New computed version $newVersion"

						bat "\"C:\\Program Files\\7-Zip\\7z.exe\" a  -r MatchingGame_$newVersion"
					}
					archiveArtifacts artifacts: 'MatchingGame/MatchingGame/bin/Release/MatchingGame*.zip'
					
					def uploadSpec = """{
					  "files": [
						{
						  "pattern": "MatchingGame/MatchingGame/bin/Release/MatchingGame*.zip/",
						  "target": "/dev-local/MatchingGame/"
						}
					 ]
					}"""
					
					def server = Artifactory.server 'my-artifactory'
					server.upload spec: uploadSpec 
				}
            }
        }
    }
}