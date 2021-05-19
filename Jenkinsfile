def gv
pipeline {
   agent any
     parameters {
	    choice (name: 'Release', choices: ['GeoLite2-City', 'GeoLite2-Country', 'GeoLite2-ASN'], description: 'Deployment of selected version')
	        booleanParam(name: 'executeS3upload', defaultValue: false, description: '')
		booleanParam(name: 'executeServerDeploy', defaultValue: true, description: '')

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
			     params.executeS3upload == true
				}
			}	
	      steps {
		      script {
			      gv.s3Download()
		      }
		
		  }
		}
   	   stage('extract') {
	      when {
		     expression {
			     params.Release == true
				}
			}	
	      steps {
		      script {
			      gv.releaseExtract()
		      }
		
		  }

		}
   	   stage('s3upload') {
	      when {
		     expression {
			     params.Release == "GeoLite2-City"
				}
			}	
	      steps {
		      script {
			      gv.releaseExtract1()
		      }
		
		  }

		}

	 }
}
