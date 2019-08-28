node {
    def pipeline
    stage('Load config and libs') {
        dir('shared-jenkins-scripts') {
            git credentialsId: '353407c1-073d-4461-90cf-08e826c8e762', url: 'https://github.com/foundworld/shared-jenkins-scripts.git'
        }
        pipeline=load('shared-jenkins-scripts/src/main/groovy/common.groovy')
    }
    stage('Build') {
        pipeline.gitclean()
        withDockerContainer(args: '-v /root/.m2:/root/.m2', image: 'maven:3-alpine') {
            sh label: 'mvn build', script: 'mvn -B -DskipTests clean package'
            withSonarQubeEnv('SonarQubeServer') {
                sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.2:sonar'
            }
        }
    }
}
