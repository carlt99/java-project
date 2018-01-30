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

	stage('Running on CentOS') {
		agent {
			label 'CentOS'
		}

		steps {
			sh 'wget http://carlthomas5.mylabserver.com/rectangles/all/rectangle_${env.BUILD_NUMBER}.jar'
			sh 'java -jar rectangle_${env.BUILD_NUMBER}.jar 3 4'
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
  }

  post {
    always {
      archiveArtifacts artifacts: 'dist/*.jar', fingerprint: true
    }
  }
}
