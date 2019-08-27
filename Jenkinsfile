pipeline {
    agent {
        docker {
            image 'maven:3-alpine' 
            args '-v /root/.m2:/root/.m2' 
        }
    }
    stages {
        def pl
        stage('Build') { 
            steps {
                sh 'mvn -B -DskipTests clean package' 
                dir('shared-jenkins-scripts') {
                    git url: 'ssh://git@github.com:foundworld/shared-jenkins-scripts.git'
                }
                pl = load('shared-jenkins-scripts/src/main/groovy/common.groovy')
                pl.gitclean()
            }
        }
        stage('SonarQube analysis') {
            steps {
                withSonarQubeEnv('SonarQubeServer') {
                    sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.2:sonar'
                }
            }
        }
    }
}
