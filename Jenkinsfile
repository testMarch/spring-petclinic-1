pipeline
	{
		agent { label 'JDK_17_Master'}
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
					sshagent(['deploy_app']) {
						sh "scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/spring-petclinicDeployTest/target/spring-petclinic-3.0.0-SNAPSHOT.jar sujata@20.51.232.169:/var/lib/tomcat9/webapps/ROOT"
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