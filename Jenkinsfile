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
		
	   stage('download') {
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
			     params.executeS3upload == true
				}
			}	
	      steps {
		      script {
			      gv.releaseExtract()
		      }
		
		  }

		}
   	   stage('s3upload') {
		   parallel {
			   stage('parallel stage 1') {
	                      when {
		                expression {
			            params.Release == "GeoLite2-City" && params.executeS3upload == true 
				    }
			         }	
	                       steps {
		                   script {
			              gv.releaseExtract1()
		                      }
		
		                    }
			   } 
			   stage('parallel stage 2') {
	                      when {
		                expression {
			            params.Release == "GeoLite2-City" && params.executeS3upload == true 
				    }
			         }	
	                       steps {
		                   script {
			              gv.releaseExtract1()
		                      }
		
		                    }
			   }
			   stage('parallel stage 3') {
	                      when {
		                expression {
			            params.Release == "GeoLite2-City" && params.executeS3upload == true 
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

	 }
}
