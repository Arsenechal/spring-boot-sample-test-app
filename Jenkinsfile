pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        echo 'build stage'
        bat './mvnw -DskipTests clean install'
        echo 'fin du build'
        archiveArtifacts '**/target/*.jar'
        bat 'docker build -t testapp:latest .'
      }
    }

    stage('test') {
      parallel {
        stage('test intÃ©gration') {
          steps {
            echo 'test d\'integration'
            bat './mvnw -Dtest=com.example.testingweb.integration.** test'
            echo 'fin integration'
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
            junit '**/target/surefire-reports/TEST-*.xml'
          }
        }

      }
    }

    stage('deploy') {
      steps {
        input(message: 'Voulez-vous continuez ?', ok: 'Allons-y')
        echo 'stage deploy'
        bat 'docker run -d -p 3030:9090 testapp:latest .'
        echo 'fin de deploimenet'
      }
    }

  }
  tools {
    maven 'maven 3.9'
    jdk 'java 11'
  }
  post {
    success {
      emailext(to: 'arnaudsenechaldu91@gmail.com', subject: "${env.BUILD_ID} - ${currentBuild.result}", body: "${env.BUILD_ID} - ${env.JENKINS_URL}")
    }

  }
}