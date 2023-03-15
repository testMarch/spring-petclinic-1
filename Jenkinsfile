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
					branch: 'release'
				}
			}
			stage('build')
			{
				steps
				{
					sh "mvn clean package"
				}
			}
			stage('sonar analysis') {
            steps {
                // performing sonarqube analysis with "withSonarQubeENV(<Name of Server configured in Jenkins>)"
                	withSonarQubeEnv('SONAR_CLOUD') {
                    sh 'mvn clean package sonar:sonar -Dsonar.organization=springpetclinic1'
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