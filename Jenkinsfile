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
          customWorkspace "$JENKINS_HOME/workspace/$BUILD_TAG"
				}
			}
			steps {
				sh "pwd"
				sh '. /tmp/venv/bin/activate && python -m pytest --junitxml=build/results.xml'
			}
		}
    stage('collect test results') {
    agent {
      docker {
        image "${params.DOCKER_IMAGE}"
        customWorkspace "$JENKINS_HOME/workspace/$BUILD_TAG"
      }
    }
      steps {
				junit "build/results.xml"
			}
    }

	}

}
