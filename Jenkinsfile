pipeline {
	agent { node { label 'Slave_for_maven' } }

	stages {
		stage('Compile Stages') {
			steps {
				withMaven(maven : 'maven_3_6_0') {
					sh 'mvn package'
				}
			}
		}

		stage('Deploy to Tomcat') {
			steps {
				sshagent(['tomcat-dev']) {
					sh 'scp -o StrictHostKeyChecking=no target/*.war root@192.168.0.122:/var/lib/tomcat/webapps'
				}
			}
		}

		stage('Restart Nginx') {
			steps {
				sshagent(['tomcat-dev']) {
					sh 'ssh -o StrictHostKeyChecking=no  root@192.168.0.122 "systemctl restart tomcat"'
				}
			}
		}
	}
}
