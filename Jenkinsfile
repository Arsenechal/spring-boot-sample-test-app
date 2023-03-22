pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        echo 'build stage'
        bat './mvnw -DskipTests clean install'
        echo 'fin du build'
      }
    }

    stage('test') {
      parallel {
        stage('test intégration') {
          steps {
            echo 'test d\'intégration'
            bat './mvnw -Dtest=com.example.testingweb.integration.** test'
            echo 'fin int�gration'
          }
        }

        stage('test fonctionnel') {
          steps {
            echo 'test fonctionnel'
            bat './mvnw -Dtest=com.example.testingweb.functional.** test'
            echo 'fin de test functionnel'
          }
        }

        stage('smoke test') {
          steps {
            echo 'smoke test'
            bat './mvnw -Dtest=com.example.testingweb.smoke.** test'
            echo 'fin smoke'
          }
        }

      }
    }

    stage('deploy') {
      steps {
        echo 'stage deploy'
      }
    }

  }
  tools {
    maven 'maven 3.9'
    jdk 'java 11'
  }
}