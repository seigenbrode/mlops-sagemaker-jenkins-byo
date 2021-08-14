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
              script {
                 def response = sh ''' 
                 aws lambda invoke --function-name """${params.LAMBDA_EVALUATE_MODEL}""" --cli-binary-format raw-in-base64-out --region us-east-1 --payload '{"EndpointName": "scikit-byo-Test","Env": "Test","S3TestData": "jenkins-scikitbyo-data", "S3Key": "test.csv"}' evalresponse.json		
              '''
		result = readFile('evalresponse.json').trim()
		if (result == '"failed"'){
		      error 'The Test Endpoint has Failed'
		      }
		      else{
		       echo 'The Test Endpoint has Succeded'
		      }
              }
            }
        }

  }
}   
