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
			   stage('Upload GeoLite2-City') {
	                      when {
		                expression {
			            params.Release == "GeoLite2-City" && params.executeS3upload == true 
				    }
			         }	
	                       steps {
		                   script {
			              gv.uploadCity()
		                      }
		
		                    }
			   } 
			   stage('Upload GeoLite2-Country') {
	                      when {
		                expression {
			            params.Release == "GeoLite2-Country" && params.executeS3upload == true 
				    }
			         }	
	                       steps {
		                   script {
			              gv.uploadCountry()
		                      }
		
		                    }
			   }
			   stage('Upload GeoLite2-ASN') {
	                      when {
		                expression {
			            params.Release == "GeoLite2-ASN" && params.executeS3upload == true 
				    }
			         }	
	                       steps {
		                   script {
			              gv.uploadASN()
		                      }
		
		                    }
			   }
			   
		   }

		}

	 }
}
