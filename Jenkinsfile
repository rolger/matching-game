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
                    //zip zipFile: 'MatchingGame.zip'
					script {
						def disk_size = bat(script: "dir", returnStdout: true)
						println("disk_size = ${disk_size}")
					
						input
						
						def commitCount = bat(script: 'git rev-list --count HEAD', returnStdout: true)
						def commitHash = bat(script: 'git rev-parse HEAD', returnStdout: true)
						println "git commit count \$commitCount commmit hash \$commitHash"

						def newVersion = "1.0." + commitCount + branch
						newVersion = newVersion.replaceAll("\\\\s", "")
						println "New computed version \$newVersion"
						
						input

						bat '"C:\\Program Files\\7-Zip\\7z.exe" a  -r MatchingGame_v${newVersion}.zip'
					}
                }
				archiveArtifacts artifacts: 'MatchingGame/MatchingGame/bin/Release/MatchingGame.zip'
            }
        }
    }
}