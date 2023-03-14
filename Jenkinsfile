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
			stage('push')
			{
				steps
				{
					sshagent(['deploy_app']) {
						sh "scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/spring-petclinicDeployTest/target/spring-petclinic-3.0.0-SNAPSHOT.jar sujata@52.146.88.86:/home/sujata/deploy"
					}
		 		}
			}
			stage('deploy')
			{
				steps
				{
					sshagent(['deploy_app']) {
						sh "echo hi"
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