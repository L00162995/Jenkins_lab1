pipeline {
  agent any
  environment {
    NEW_VERSION = '1.0'
  }
  stages {

    stage('Initialize')  {
      steps {
        echo 'Initialize Steps'
        sh 'mvn -version'
      }
    }
    stage('Build') {
      steps {
        echo 'maven build'
        sh 'mvn -B -DskipTests clean package'
        echo "Building version: ${NEW_VERSION}"
      }
    }

    stage('Maven Test') { 
          when {
            expression {
              BRANCH_NAME == 'dev'
            }
          }

      steps {
        sh 'mvn test'
      }
    }
    stage('Deploy ') {

      parallel {
        stage('Deploy in Dev') {
          when {
            branch 'dev'
          }
          
          steps {
            echo 'Deploying in DEV with test'
          }
        }

        stage('Deploy PREPROD') {
          when {
            branch 'PREPROD'
          }          
          steps {
            echo 'Deploying in PREPROD'
          }
        }

        stage('DeployPROD') {
          when {
            branch 'main'
          }
          steps {
            echo 'Deploying in PROD'
          }
        }

      }
    }

    stage('Test') { ///  TEST WILL ONLY EXCUTE IF THE CURRENT BRANCH IS DEV
      steps {
       echo 'test successful'
      }
    }

    stage('Archive') {
      steps {
        archiveArtifacts(allowEmptyArchive: true, artifacts: 'target/*.jar')
      }
    }
  }
}