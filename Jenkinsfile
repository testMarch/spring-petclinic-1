pipeline
	{
		agent { label 'node1'}
        
		stages
		{
			stage('vcs')
			{
				steps
				{
					git url: 'https://github.com/March2023Sujata/spring-petclinic.git',
					branch: 'develop'
				}
			}
			stage('package')
			{
				steps
				{
					sh "mvn clean package"
				}
			}
			stage('post build')
			{
				steps
				{
					archiveArtifacts artifacts: '**/target/spring-petclinic-3.0.0-SNAPSHOT.jar',
									 onlyIfSuccessful: true
					junit testResults: '**/surefire-reports/TEST-*.xml'                 
				}
			}
		}
	}