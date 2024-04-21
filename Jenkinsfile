node {
  stage('(SCM) Checkout') {
    checkout scm
  }
  stage('(SAST) SonarQube Analysis') {
    def scannerHome = tool 'SonarScanner';
    withSonarQubeEnv() {
      sh "${scannerHome}/bin/sonar-scanner"
    }
  }
  stage('(SCA) OWASP Dependency Check') {
      steps {
        dependencyCheck additionalArguments: ''' 
                    -o './'
                    -s './'
                    -f 'ALL' 
                    --prettyPrint''', odcInstallation: 'OWASP Dependency Check'
        
        dependencyCheckPublisher pattern: 'dependency-check-report.xml'
      }
    }
}
