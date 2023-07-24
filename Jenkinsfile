pipeline {
  agent any
 
    stages {

  stage ('Checkout SCM'){
        steps {
          checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'Git', url: 'https://github.com/Prasannamugundhan/gradle-example.git']]])
      }
   }
	  //Build stage
	stage ('Build')  {
	    steps {
            sh "gradlew tasks"
            sh "gradle build"
          }
            
   }
   

    stage ('Artifactory configuration') {
            steps {
                rtServer (
                    id: "jfrog",
                    url: "http://jfrog.ijarah.loc:8082/ui/admin/artifactory",
                    credentialsId: "jfrog",
		    bypassProxy: true
                )

                rtGradleDeployer (
                    id: "GRADLE_DEPLOYER",
                    serverId: "jfrog",
                    //repo: "android_artifact-libs-release-local",
                    //repo: "android_artifact-snapshot-local"
                )

                rtGradleResolver (
                    id: "GRADLE_RESOLVER",
                    serverId: "jfrog",
                    //repo: "default-gradle-virtual",
                    //repo: "default-gradle-virtual"
                )
            }
    }

    stage ('Deploy Artifacts') {
            steps {
                rtGradleRun (
                    //tool: "gradle", // Tool name from Jenkins configuration
                    //pom: 'app/pom.xml',
                    //goals: 'gradle install',
                    deployerId: "GRADLE_DEPLOYER",
                    resolverId: "GRADLE_RESOLVER"
                )
         }
    }
    }
}
