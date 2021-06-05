def gv
pipeline {
   agent any
     parameters {
	    choice (name: 'Release', choices: ['GeoLite2-City', 'GeoLite2-Country', 'GeoLite2-ASN'], description: 'Deployment of selected Release')
	        booleanParam(name: 'executeS3upload', defaultValue: false, description: '')
		booleanParam(name: 'executeServerDeploy', defaultValue: true, description: 'Select one of the server ')
	     choice (name: 'Server', choices: ['api-prod-beta-1', 'api-prod-beta-2', 'api-msvcs-grp-g-01', 'api-msvcs-grp-g-02'], description: '')

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
			   stage('S3 GeoLite2-City') {
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
			   stage('S3 GeoLite2-Country') {
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
			   stage('S3 GeoLite2-ASN') {
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
   	   stage('deploy') {
	      when {
		     expression {
			     params.executeServerDeploy == true
				}
			}	
	      steps {
		      script {
			      gv.serverDeploy()
		      }
		
		  }

		}
		 
   	   stage('revoke') {
	      when {
		     expression {
			     params.executeServerDeploy == true
				}
			}	
	      steps {
		      script {
			      gv.revokeDeploy()
		      }
		
		  }

		}

	 }
}
