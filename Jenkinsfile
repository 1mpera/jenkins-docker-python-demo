pipeline {
	agent any
	options {
		buildDiscarder(logRotator(numToKeepStr: '5'))
	}
	parameters {
		string(name: 'DOCKER_IMAGE', defaultValue: 'maxsum:build', description: '')
		string(name: 'LATEST_BUILD_TAG', defaultValue: 'build-latest', description: '')
	}
	stages {
    stage("build Docker image") {
      steps {
        script {
          docker.build("${params.DOCKER_IMAGE}")
        }
      }
    }
		stage("test") {
			agent {
				docker {
					image "${params.DOCKER_IMAGE}"
				}
			}
			steps {
				sh "pwd"
				sh '. /tmp/venv/bin/activate && python -m pytest --junitxml=build/results.xml'
			}
		}
		stage('collect test results') {
			steps {
				junit "build/results.xml"
			}
    }

	}

}
