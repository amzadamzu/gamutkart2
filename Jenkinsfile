pipeline {
    agent any

	tools {
		jdk 'jdk8'
	}
//	environment {
//		M2_INSTALL = "/usr/bin/mvn"
//	}

    stages {
        stage('Clone-Repo') {
	    steps {
	        checkout scm
	    }
        }

        stage('Build') {
            steps {
                sh 'mvn install -Dmaven.test.skip=true'
            }
        }
		
        stage('Unit Tests') {
            steps {
                sh 'mvn compiler:testCompile'
                sh 'mvn surefire:test'
                junit 'target/**/*.xml'
            }
        }

        stage('Deployment') {
            steps {
                sh 'sshpass -p "giri" scp target/gamutkart-1.1-RELEASE.war giri@172.17.0.3:/home/giri/project/apache-tomcat-9.0.70/webapps'
                sh 'sshpass -p "giri" ssh giri@172.17.0.3 "/home/giri/project/apache-tomcat-9.0.70/bin/startup.sh"'
            }
        }
    }
}
