node{

   stage("Git Checkout"){
	   echo "Checkout code from Git Repo"
       git credentialsId: 'gitpasswd', url: 'https://github.com/krisha2705/spring-boot-mongo-docker.git'
   }
	
   stage("Maven Build"){
	   echo "Build the code"
       def mavenHome = tool name: "Maven", type: "maven"
       def mavenCMD = "${mavenHome}/bin/mvn "
       sh "${mavenCMD} clean package"
   }
	
   stage("Maven Test"){
	   echo "Testing code"
       def mavenHome = tool name: "Maven", type: "maven"
       def mavenCMD = "${mavenHome}/bin/mvn "
       sh "${mavenCMD} test"
   }
	
   stage("Build Docker Image"){
	   echo "Build Docker image with the given tag"
       sh "docker build -t krisha2705/spring-boot-m ."
   }
	
   stage("Docker Push"){
	   echo "Push docker image to docker hub"
       withCredentials([string(credentialsId: 'DOCKER_CREDEDENTIAL', variable: 'DOCKER_CREDEDENTIAL')]) {
       sh "docker login -u krisha2705 -p ${DOCKER_CREDEDENTIAL}"
     }
       sh "docker push krisha2705/spring-boot-m "
   }
	
   stage("Deploy Application in K8s Cluster"){
	   echo "Deploy application to kubernetes container"
       kubernetesDeploy(
           configs: "springBootMongo.yml",
           kubeconfigId: "KUBERNETES_CLUSTER_CONFIG",
           enableConfigSubstitution: true
           )
   }
}
