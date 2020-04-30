node {
	def server = Artifactory.server 'artifactory' 
    def rtMaven = Artifactory.newMavenBuild()
    def buildInfo
    def mvnHome
    jdk = tool name: 'JAVA8'
    env.JAVA_HOME = "${jdk}"
    
   stage ('Clone') {			
	git branch: 'master', url: "https://github.com/ChiragMakkar13/MavenProject.git"
		}
	stage ('Artifactory configuration') {
        mvnHome = tool 'mavenhome'
        rtMaven.tool = 'mavenhome' // Tool name from Jenkins configuration
        rtMaven.deployer releaseRepo: 'libs-release-local', snapshotRepo: 'libs-snapshot-local', server: server
        rtMaven.resolver releaseRepo: 'libs-release', snapshotRepo: 'libs-snapshot', server: server
        buildInfo = Artifactory.newBuildInfo()
        buildInfo.env.capture = true
    }		
	stage ('Maven Build') {
	    rtMaven.run pom: 'pom.xml', goals: 'clean install', buildInfo: buildInfo
    }
   

	stage('Results') {
      junit '**/target/surefire-reports/TEST-*.xml'
      archive 'target/*.jar'
   }	
 
	stage ('Artifactory Build Publish') {
     server.publishBuildInfo buildInfo
    }
	stage('Docker Image Build'){
	   sh "sudo docker build -t chiragmakkar13/mavendemo:Dockerfile ."
          }

   
	
	
	
 }







