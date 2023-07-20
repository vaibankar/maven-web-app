node{
    
    stage('Clone repo'){
        git 'https://github.com/hackerboypk/maven-web-app.git'
    }
    
    stage('Maven Build'){
        def mavenHome = tool name: "Maven-3.9.3", type: "maven"
        def mavenCMD = "${mavenHome}/bin/mvn"
        sh "${mavenCMD} clean package"
    }
     stage('SonarQube analysis') {       
        withSonarQubeEnv('Sonar-Server-7.8') {
       	sh "mvn sonar:sonar"    	
       }
    }
    stage('upload war to nexus'){
	nexusArtifactUploader artifacts: [[artifactId: '01-maven-web-app', classifier: '', file: 'target/01-maven-web-app.war', type: 'war']], credentialsId: 'Nexus-credentials', groupId: 'in.ashokit', nexusUrl: '54.144.93.132:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'ashokit-snapshot', version: '1.0-SNAPSHOT'
}
    stage('Deploy to tomcat'){
        sshagent(['tomcat-server-agent']) {
    sh 'scp -o StrictHostKeyChecking=no target/01-maven-web-app.war ubuntu@52.90.3.24:/opt/tomcat/webapps'
}
    }
}
