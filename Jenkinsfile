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
				sshagent(credentials: ['11c8c96f-0a39-4893-b03e-0ac3c796871e'], ignoreMissing: true) {
					sh 'scp -o StrictHostKeyChecking=no target/*.war -l cloudbees 192.168.1.106 root -a /var/lib/tomcat/webapps/'
				}
			}
		}

		stage('Restart Nginx') {
			steps {
				sshagent(credentials: ['11c8c96f-0a39-4893-b03e-0ac3c796871e'], ignoreMissing: true) {
					sh 'ssh -o StrictHostKeyChecking=no -l cloudbees 192.168.1.106 root -a "systemctl restart tomcat"'
				}
			}
		}
	}
}
