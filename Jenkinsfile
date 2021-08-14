pipeline {

    agent any

    environment {
        AWS_ECR_LOGIN = 'true'
        DOCKER_CONFIG= "${params.JENKINSHOME}"
        END_POINT = 'scikit-byo'
    }

    stages {
        stage("Checkout") {
            steps {
               checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/chethancmk/mlops-sagemaker-jenkins-byo']]])
            }
        }

      
      stage("TestEvaluate") {
            steps { 
			withAWS(region:'us-east-1') {
			      invokeLambda(
					functionName: "${params.LAMBDA_EVALUATE_MODEL}" ,
					payload: [ "EndpointName": "${env.END_POINT}-Test","Env": "Test", "S3TestData": "${params.S3_TEST_DATA}", "S3Key": "test.csv" ]
					)	
				}
			   println "This is a test"
              }
            }

  }
}   
