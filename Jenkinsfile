node {
    def mavenHome = tool name: 'maven3.8.2'
    stage ('1.Clone') {
        git credentialsId: 'Git_Credential', url: 'https://github.com/teamconquerors/maven-web-app.git'
    }
    stage ('2.MavenBuild') {
        sh "${mavenHome}/bin/mvn clean package"
    }
    stage ('3.CodeQuality') {
        sh "${mavenHome}/bin/mvn sonar:sonar"
    }
    stage ('3.UploadArtifacts') {
        sh "${mavenHome}/bin/mvn deploy"
    }
    stage ('5.Deploy-UAT') {
        deploy adapters: [tomcat9(credentialsId: 'Tomcat', path: '', url: 'http://192.168.1.244:8080/')], contextPath: null, war: 'target/*.war'
    }
    stage ('EmailNotification') {
        emailext body: '''Hello Everyone,
        
        Build from eBay pipeline project
        
        Landmark Technologies''', subject: 'Build Status', to: 'Developers'
    }
    stage('Approval') {
        timeout(time:8, unit: 'HOURS') {
            input massage: 'Please verify and approve'
        }
    }
    stage('Prod-Deploy') {
        deploy adapters: [tomcat9(credentialsId: 'Tomcat', path: '', url: 'http://192.168.1.244:8080/')], contextPath: null, war: 'target/*.war'
    }
}
