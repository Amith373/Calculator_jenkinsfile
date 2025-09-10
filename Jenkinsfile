pipeline{
	agent none
	stages{
		stage('Build'){
		agent { label 'master' }
			steps{
				git branch : 'master', url : 'https://github.com/Agasthyahm/calendar.git'
			}
		}
		stage('Deploy into both server'){
			parallel{
				stage('deploy to server1'){
				agent { label 'master' }
					steps{
						sh ''' ssh ec2-user@172.31.20.104 "
							scp ubuntu@172.31.46.234:/home/ec2-user/jenkins/workspace/tomcat-deployment/Calendar.war . 
							sudo cp Calendar.war /opt/tomcat/webapps/
							sudo systemctl restart tomcat "
					'''
						
					}
				}
				stage('deploy to server2'){
				agent { label 'master' }
					steps{
						sh ''' ssh ubuntu@172.31.27.245 "
							scp ubuntu@172.31.46.234:/home/ec2-user/jenkins/workspace/tomcat-deployment/Calendar.war .
							sudo cp  Calendar.war /opt/tomcat/webapps/
							sudo systemctl restart tomcat  "
					'''
					}
				}
			}
		}
		stage('Test'){
		agent { label 'master' }
			steps{
				sh ''' 
						echo -e "Testing for server1 \n"
						curl -Is http://34.202.231.0:8080/Calendar/Calendar.html

						echo -e "\n Testing for server2"
						curl -Is http://52.90.147.94:8080/Calendar/Calendar.html
 '''
			}
		}
	}
}
