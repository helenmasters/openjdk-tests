#!groovy
pipeline {
    agent any
    environment {
                JAVA_BIN='$WORKSPACE/openjdkbinary/j2sdk-image/jre/bin'
                JCL_VERSION='current'
                OPENJDK_TEST='$WORKSPACE/openjdk-test'
                JAVA_VERSION='SE80'
                SPEC='linux_x86-64_cmprssptrs'
                }
    stages {
        stage('Setup') {
            steps {
            	sh 'chmod 755 $OPENJDK_TEST/maketest.sh'
                sh 'chmod 755 $OPENJDK_TEST/get.sh'
                sh '$OPENJDK_TEST/get.sh $WORKSPACE $OPENJDK_TEST'
                }
        }
        stage('BuildAndRun') {
            steps {
                echo 'Building and running tests...'
                sh 'printenv'
                sh '$OPENJDK_TEST/maketest.sh $OPENJDK_TEST'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
                step([$class: "TapPublisher", testResults: "**/*.tap"])
                archiveArtifacts artifacts: '**/*.tap', fingerprint: true
            }
        }
    }
    post {
 		always {
 			deleteDir()
 		}
 	}
    
}
