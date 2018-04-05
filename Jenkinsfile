pipeline {
  agent {
    docker {
      image 'maven:alpine'
      args '-v /var/run/docker.sock:/var/run/docker.sock -v $HOME/.m2:/root/.m2'
    }
    
  }
  stages {
    stage('Build') {
      steps {
        sh 'mvn clean package -DskipTests=true -f $PWD/sonarqube-scanner-maven/pom.xml'
        archiveArtifacts(allowEmptyArchive: true, artifacts: '**/target/*.jar', fingerprint: true, onlyIfSuccessful: true, defaultExcludes: true)
      }
    }
    stage('Unit test') {
      steps {
        sh 'mvn test -f $PWD/sonarqube-scanner-maven/pom.xml'
        junit(testResults: '**/target/surefire-reports/*.xml', allowEmptyResults: true)
      }
    }
  }
}