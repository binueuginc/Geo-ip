def gv
pipeline {
   agent any
     parameters {
	    choice (name: 'RELEASE', choices: ['GeoLite2-City', 'GeoLite2-Country', 'GeoLite2-ASN'], description: 'Deployment of selected Release')
	        booleanParam(name: 'executeS3upload', defaultValue: false, description: '')
		booleanParam(name: 'executeDeployGeoip from S3', defaultValue: false, description: 'Please select one of the following server')
	        booleanParam(name: 'api-prod-beta-1', defaultValue: false, description: '')
	        booleanParam(name: 'api-prod-beta-2', defaultValue: false, description: '')
	        booleanParam(name: 'api-msvcs-grp-g-01', defaultValue: false, description: '')
	        booleanParam(name: 'api-msvcs-grp-g-02', defaultValue: false, description: '')

		}
	environment {
		DOCKERRUN = "docker run -p 8080:8080 my-app binueuginc/sample-myapp:${params.VERSION} " 
		DOCKERCLEAN = "docker images  | awk '{print \$1 \":\" \$2}' | xargs docker rmi -f"
                TASK_FAMILY = "sample-myapp-task"
                SERVICE_NAME = "sample-myapp-service"
                NEW_DOCKER_IMAGE = "sample-myapp:${params.VERSION}"
                CLUSTER_NAME = "sample-myapp-cluster"
                OLD_TASK_DEF = "aws ecs describe-task-definition --task-definition ${env.TASK_FAMILY} --output json"
                NEW_TASK_DEF= "echo ${env.OLD_TASK_DEF} | jq --arg NDI ${env.NEW_DOCKER_IMAGE} '.taskDefinition.containerDefinitions[0].image=$NDI'"
                FINAL_TASK = "echo ${env.NEW_TASK_DEF} | jq '.taskDefinition|\{family: .family, volumes: .volumes, containerDefinitions: .containerDefinitions\}'"
                
	}
         stages {
	    stage('init'){
	     steps {
		   script {
		   gv = load "groovy.script"
		      } 
		   }
	    }
		
	   stage('build') {
	      when {
		     expression {
			     BRANCH_NAME == "${params.BRANCH}" && params.executeBuild == true 
				}
			}	
	      steps {
		      script {
			      gv.dockerBuild()
		      }
		
		  }
		}
		stage('test'){
		   when {
		      expression {
			     params.executeTest == true 
				 }
		   }
		   steps{
			   script {
		               gv.testApp()
			 }
		     }
		}
		 stage('dockerPush'){
		   when {
		      expression {
			      params.executeDockerPush == true 
				 }
		    }
		   steps{
			   script {
		               gv.dockerPush()
			   }
                   }
		}
		stage('dockerTestDeploy'){
		   when {
		      expression {
			      params.executeTestDeploy == true && BRANCH_NAME == "${params.BRANCH}"
				 }
		    }
		   steps{
			   script {
		               gv.dockerDeployApp()
			   }
             }
		}
		stage('awsEcsDeploy'){
		   when {
		      expression {
			      params.executeAwsEcsDeploy == true 
				 }
		    }
		   steps{
			   script {
		               gv.dockerEcsDeployApp()
			   }
             }
		}
  
	 }
}
