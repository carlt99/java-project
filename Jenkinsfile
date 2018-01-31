pipeline {
  agent none
  
  stages {
	stage('build') {
		agent {
			label 'Apache'
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

	stage('Unit Tests') {
		agent {
			label 'Apache'
		}

		steps {
			sh 'ant -f test.xml -v'
                        junit 'reports/result.xml'
		}
	}

	stage('deploy') {
		agent {
			label 'Apache'
		}

		steps {
			sh "cp dist/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/all"
		}
	}

	stage('Running on CentOS') {
		agent {
			label 'Master'
		}

		steps {
			sh "wget http://carlthomas5.mylabserver.com/rectangles/all/rectangle_${env.BUILD_NUMBER}.jar -O rectangle_${env.BUILD_NUMBER}.jar"
			sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 3 4"
		}
	}

	stage('Test on Debian') {
		agent {
			docker 'openjdk:8u121-jre'
		}

		steps {
			sh "wget http://carlthomas5.mylabserver.com/rectangles/all/rectangle_${env.BUILD_NUMBER}.jar -O rectangle_${env.BUILD_NUMBER}.jar"
			sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 3 4"
		}
	}

	stage('Promote to Green') {
		steps {
			sh "cp /var/www/html/rectangles/all/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/green/rectangle_${env.BUILD_NUMBER}.jar"
wget http://carlthomas5.mylabserver.com/rectangles/all/rectangle_${env.BUILD_NUMBER}.jar -O rectangle_${env.BUILD_NUMBER}.jar"
		}
	}	
  }

}
