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
			stage('build')
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
			stage('push')
			{
				steps
				{
					sshagent(['deploy_app']) {
						sh "scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/spring-petclinicDeployTest/target/spring-petclinic-3.0.0-SNAPSHOT.jar sujata@52.146.88.86:/home/sujata"
					}
		 		}
			}
			stage('deploy')
			{
				steps
				{
					sshagent(['deploy_app']) {
						
						sh "ssh sujata@52.146.88.86 java -jar spring-petclinic-3.0.0-SNAPSHOT.jar"
					}
		 		}
			}
			
		}
	}