pipeline {
    agent {
        docker {
            image 'maven:3-alpine' 
            args '-v /root/.m2:/root/.m2' 
        }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'mvn -B -DskipTests clean package' 
            }
        }
    }
    stages {
        stage('SonarQube analysis') {
            def scannerHome = tool name:'SonarQube Scanner';
            withSonarQubeEnv('SonarQubeServer') {
                sh "${scannerHome}/bin/sonar-scanner"
            }
        }
    }
}
