#!/bin/groovy
pipeline{
agent any
       stages {
	 
	  stage("Git Checkout"){
               steps{
       git credentialsId: 'gitpasswd', url: 'https://github.com/krisha2705/spring-boot-mongo-docker.git'
   }
	  }
	   stage("Maven Test"){
		   steps{
			   withMaven(maven: 'Maven'){
                sh "mvn clean test"
			   }
		   }
	   }
	   stage("Maven Package"){
		    steps{
			    withMaven(maven: 'Maven'){
			    
		   sh "mvn clean package"
			    }
		    }
	    }
	   stage("Build Docker Image"){
		    steps{
			    
		   sh "docker build -t krisha2705/spring-boot-m ."
		    }
	    }
	   stage("Docker Push"){
		       steps{
			       
		   withCredentials([string(credentialsId: 'DOCKER_CREDEDENTIAL', variable: 'DOCKER_CREDEDENTIAL')]) {
			   
                   sh "docker login -u krisha2705 -p ${DOCKER_CREDEDENTIAL}"
                   sh "docker push krisha2705/spring-boot-m "	
		   }
		       }
	   }
		       
	   stage("Deploy Application in K8s Cluster"){
			  steps{
           kubernetesDeploy(
           configs: "springBootMongo.yml",
           kubeconfigId: "KUBERNETES_CLUSTER_CONFIG",
           enableConfigSubstitution: true
           )
  			 }      
		  }
	       }
}
