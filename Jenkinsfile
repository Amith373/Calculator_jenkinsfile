pipeline{
	agent none
	stages{
		stage('Build'){
		agent { label 'slave' }
			steps{
				git branch : 'master', url : 'https://github.com/Amith373/Calendar.git'
			}
		}
				stage('deploy to server1'){
				agent { label 'slave' }
					steps{
						sh ''' ssh ubuntu@172.31.27.245 "
							scp ubuntu@172.31.17.32:/var/lib/jenkins/workspace/tomcat-deployment/Calendar.war .
							sudo cp  Calendar.war /opt/tomcat/webapps/
							sudo systemctl restart tomcat  "
					'''
					}
				}
		stage('Test'){
		agent { label 'slave' }
			steps{
				sh ''' 
						echo -e "\n Testing for server1"
						curl -Is http://98.88.249.239:8080/Calendar/Calendar.html
                   '''
			}
		}
	}
}	
