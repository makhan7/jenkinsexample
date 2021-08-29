pipeline {
	agent any

	
options {
		buildDiscarder(logRotator(numToKeepStr: '10'))
	}

	


	stages {
        

        stage('Test') {
            when {
                environment name: 'RUN_TESTS', value: 'true'
            }
            steps {
                ctest 'InSearchPath'
            }
        }

        stage('Analyse') {
            when {
                environment name: 'RUN_ANALYSIS', value: 'true'
            }
            steps {
                sh label: '', returnStatus: true, script: 'cppcheck . --xml --language=c++ --suppressions-list=suppressions.txt 2> cppcheck-result.xml'
                publishCppcheck allowNoReport: true, ignoreBlankFiles: true, pattern: '**/cppcheck-result.xml'
            }
        }

        stage('Deploy') {
            when {
                environment name: 'DEPLOY', value: 'true'
            }
            steps {
                sh label: '', returnStatus: true, script: '''cp jenkinsexample ~
                cp test/testPro ~'''
            }
        }
	}
}
