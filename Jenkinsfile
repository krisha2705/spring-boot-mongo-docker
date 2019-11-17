node{

   stage("Git Checkout"){
       git credentialsId: 'gitpasswd', url: 'https://github.com/krisha2705/spring-boot-mongo-docker.git'
   }
   stage("Maven Build"){
       def mavenHome = tool name: "Maven", type: "maven"
       def mavenCMD = "${mavenHome}/bin/mvn "
       sh "${mavenCMD} clean package"
   }
   stage("Maven Test"){
       def mavenHome = tool name: "Maven", type: "maven"
       def mavenCMD = "${mavenHome}/bin/mvn "
       sh "${mavenCMD} test"
   }
   stage("Build Docker Image"){
       sh "docker build -t krisha2705/spring-boot-m ."
   }
   stage("Docker Push"){
       withCredentials([string(credentialsId: 'DOCKER_CREDEDENTIAL', variable: 'DOCKER_CREDEDENTIAL')]) {
       sh "docker login -u krisha2705 -p ${DOCKER_CREDEDENTIAL}"
     }
       sh "docker push krisha2705/spring-boot-m "
   }
   stage("Deploy Application in K8s Cluster"){
       kubernetesDeploy(
           configs: "springBootMongo.yml",
           kubeconfigId: "KUBERNETES_CLUSTER_CONFIG",
           enableConfigSubstitution: true
           )
   }
}
	 
	  /**
    stage("Deploy To Kuberates Cluster"){
        sh 'kubectl apply -f pringBootMongo.yml'
      } **/
     

