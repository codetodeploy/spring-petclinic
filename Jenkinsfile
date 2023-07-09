pipeline {
  agent any
  stages {
    stage('compile') {
      steps {
        sh './mvnw clean compile'
      }
    }

    stage('Static Analysis') {
      steps {
        sh '''./mvnw sonar:sonar \\
  -Dsonar.projectKey=Petclinic \\
  -Dsonar.projectName=\'Petclinic\' \\
  -Dsonar.host.url=http://3.110.235.0:9000 \\
  -Dsonar.token=sqp_51ddf0cdda92d6c17e01efc61d65fc3485bb76c8'''
      }
    }

    stage('unit test') {
      steps {
        sh './mvnw "-Dtest=**/petclinic/*/*.java" test'
      }
    }

    stage('package') {
      steps {
        sh '''./mvnw package -DskipTests=true
'''
      }
    }

    stage('Deploy') {
      parallel {
        stage('Deploy') {
          steps {
            sh './mvnw spring-boot:run </dev/null &>/dev/null &'
          }
        }

        stage('integration and performance test') {
          steps {
            sh './mvnw verify'
          }
        }

      }
    }

  }
}