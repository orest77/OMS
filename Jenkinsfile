pipeline {
	agent  {
    	node {
        	label 'Slave_for_maven'
        	customWorkspace '/home/orest'
    }

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
				sshagent(['3c1c5232-5850-474e-813d-d882091e30b8']) {
					sh 'scp -o StrictHostKeyChecking=no target/*.war orest@192.168.0.122:/var/lib/tomcat/webapps/' 
				}
			}
		}

		stage('Restart Nginx') {
			steps {
				sshagent(['3c1c5232-5850-474e-813d-d882091e30b8']) {
					sh 'ssh -o StrictHostKeyChecking=no  orest@192.168.0.122 "systemctl restart tomcat"' 
				}
			}
		}
	}
}
