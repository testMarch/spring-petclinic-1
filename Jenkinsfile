pipeline
	{
		agent { label 'node1'}
        triggers {
            pollSCM('* 23 * * 1-5')
        }
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
			stage('deploy')
			{
				steps
				{
					sshagent(['deploy_ser']) {
						sh "scp  -o StrictHostKeyChecking=no **/target/spring-petclinic-3.0.0-SNAPSHOT.jar sujata@74.235.162.232:/var/lib/tomcat9/webapps/ROOT/"
					}
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