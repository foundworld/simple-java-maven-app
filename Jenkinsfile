node {
    def pipeline
    stage('Load config and libs') {
        dir('shared-jenkins-scripts') {
            git credentialsId: '353407c1-073d-4461-90cf-08e826c8e762', url: 'https://github.com/foundworld/shared-jenkins-scripts.git'
        }
        pipeline=load('shared-jenkins-scripts/src/main/groovy/common.groovy')
        pipeline.gitClean()
    }
    stage('Build') {
        pipeline.gitCommitInfo()
        sh label: 'mvn build', script: 'mvn -B -DskipTests clean package'
    }
    stage('SonarQube analysis') {
        withSonarQubeEnv('SonarQubeServer') {
            sh label: 'sonar exec', script: 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.2:sonar'
        }
    }
}
