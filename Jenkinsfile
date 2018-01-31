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
			sh "mkdir -p /var/www/html/rectangles/all/${env.BRANCH_NAME}"
			sh "cp dist/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/all/${env.BRANCH_NAME}/"
		}
	}

	stage('Running on CentOS') {
		agent {
			label 'Master'
		}

		steps {
			sh "wget http://carlthomas5.mylabserver.com/rectangles/all/${env.BRANCH_NAME}/rectangle_${env.BUILD_NUMBER}.jar -O rectangle_${env.BUILD_NUMBER}.jar"
			sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 3 4"
		}
	}

	stage('Test on Debian') {
		agent {
			docker 'openjdk:8u121-jre'
		}

		steps {
			sh "wget http://carlthomas5.mylabserver.com/rectangles/all/${env.BRANCH_NAME}/rectangle_${env.BUILD_NUMBER}.jar -O rectangle_${env.BUILD_NUMBER}.jar"
			sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 3 4"
		}
	}

	stage('Promote to Green') {
		agent {
			label 'Apache'
		}

		when {
			branch 'master'
		}

		steps {
			sh "cp /var/www/html/rectangles/all/${env.BRANCH_NAME}/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/green/rectangle_${env.BUILD_NUMBER}.jar"
		}
	}

	stage('Promote Development Branch to Master') {
		agent {
			label 'Apache'
		}

		when {
			branch 'development'
		}

		steps {
			echo "Stashing any Local Changes"
			sh "git stash"
			echo "Checking out dev branch"
			sh "git checkout development"
			echo "Checking out master branch"
			sh "git checkout master"
			echo "Merging development into master branch"
			sh "git merge development"
			echo "Pushing to origin master"
			sh "git push origin master"
		}
	}	
  }

}
