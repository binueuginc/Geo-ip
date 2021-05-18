def gv
pipeline {
   agent any
     parameters {
	    choice (name: 'VERSION', choices: ['1.0.1', '1.2.0', '1.3.0'], description: 'Deployment of selected version')
	        booleanParam(name: 'executeBuild', defaultValue: false, description: '')
		booleanParam(name: 'executeTest', defaultValue: true, description: '')

		}
	environment {
                TASK_FAMILY = "sample-myapp-task"
                SERVICE_NAME = "sample-myapp-service"               
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
			     BRANCH_NAME == "params.executeBuild == true 
				}
			}	
	      steps {
		      script {
			      gv.dockerBuild()
		      }
		
		  }
		}
	 }
}
