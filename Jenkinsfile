pipeline {
    agent any

    stages {
        stage('Clone the Git') {
            steps {
                git 'https://github.com/Pratikshadukare/jsonification.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Unit Tests') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Quality Checks') {
            steps {
                sh 'mvn checkstyle:checkstyle'
                sh 'mvn spotbugs:check'
                sh 'mvn pmd:check'
            }
        }
        stage('SonarQube analysis') {
            environment {
                scannerHome = tool 'sonarqube'
            }
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh "${scannerHome}/bin/sonar-scanner \
                    -D sonar.login=admin \
                    -D sonar.password=2022bali@123 \
                    -D sonar.projectKey=javaScanner \
                    -D sonar.projectName=javaScanner \
                    -D sonar.projectVersion=1.0 \
                    -D sonar.sources=. \
                    -D sonar.java.binaries=target/classes \
                    -D sonar.exclusions=vendor/**,resources/** \
                    -D sonar.host.url=http://13.126.56.81:9000/"
                }
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/target/*.jar'
            junit 'target/surefire-reports/**/*.xml'
        }
        success {
            echo 'Build successful!'
        }
        failure {
            echo 'Build failed.'
        }
    }
}



=======================

pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Unit Tests') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Quality Checks') {
            steps {
                sh 'mvn checkstyle:check pmd:pmd findbugs:findbugs'
            }
        }
        stage('SonarQube analysis') {
            environment {
                scannerHome = tool 'sonarqube'
            }
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh "${scannerHome}/bin/sonar-scanner \
                    -D sonar.login=admin \
                    -D sonar.password=2022bali@123 \
                    -D sonar.projectKey=javaScanner \
                    -D sonar.projectName=javaScanner \
                    -D sonar.projectVersion=1.0 \
                    -D sonar.sources=. \
                    -D sonar.java.binaries=target/classes \
                    -D sonar.exclusions=vendor/**,resources/** \
                    -D sonar.host.url=http://13.126.56.81:9000/"
                }
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/target/*.jar'
            junit 'target/surefire-reports/**/*.xml'
        }
        success {
            echo 'Build successful!'
        }
        failure {
            echo 'Build failed.'
        }
    }
}


node {
  stage('Clone the Git') {
    git 'https://github.com/Pratikshadukare/jsonification.git'
  }
  stage('SonarQube analysis') {
    def scannerHome = tool 'sonarqube';
    withSonarQubeEnv('sonarqube') {
      sh "${scannerHome}/bin/sonar-scanner \
      -D sonar.login=admin \
      -D sonar.password=2022bali@123 \
      -D sonar.projectKey=javaScanner \
      -D sonar.exclusions=vendor/**,resources/**,**/*.java \
      -D sonar.host.url=http://192.168.1XX.XXX:9000/"
    }
  }
  stage('Quality Gates'){
      
     timeout(time: 1, unit: 'HOURS') {
    def qg = waitForQualityGate() 
    if (qg.status != 'OK') {
      error "Pipeline aborted due to quality gate failure: ${qg.status}"
    }
  }
      
  }
}
