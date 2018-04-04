pipeline {
	agent none
	
	stages {
		stage('Unit Tests') {
			agent {
				label 'master'
			}
			steps {
				sh 'ant -f test.xml -v'
				junit 'reports/result.xml'
			}
		}
		stage('build') {
			agent {
				label 'master'
			}
			steps {
				sh 'ant -f build.xml -v'
			}
			post {
				success {
					archiveArtifacts artifacts: 'dist/*.jar', fingerprint: true
				}
			}
		}
		stage('deploy') {
			agent {
				label 'master'
			}
			steps {
				sh 'cp dist/rectangle.jar /var/www/html/rectangles/all/'
			}
		}
		stage('Test on Debian') {
			agent {
				docker 'openjdk:8u121-jre'
			}
			steps {
				sh "wget http://172.20.128.161/rectangles/all/rectangle.jar"
				sh "java -jar rectangle.jar 3 4"
			}
		}
		stage('Promote to Green') {
			agent {
				label 'master'
			}
			steps {
				sh "cp /var/www/html/rectangles/all/rectangle.jar /var/www/html/rectangles/green"
			}
		}
	}
	
	
}