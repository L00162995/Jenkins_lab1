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

    stage('JUnit Test') { /// jUNIT TEST WILL ONLY EXCUTE IF THE CURRENT BRANCH IS DEV
      when { 
        expression {
          BRANCH_NAME == 'dev'
        }
      }
      steps {
        echo 'test successfuly done'
      }
    }
    stage('Deploy ') {
      parallel {
        stage('Deploy Dev') {
          steps {
            echo 'Deploying in DEV with test'
          }
        }

        stage('Deploy PREPROD') {
          steps {
            echo 'Deploying in PREPROD'
          }
        }

        stage('DeployPROD') {
          steps {
            echo 'Deploying in PROD'
          }
        }

      }
    }

    stage('Test') {
      steps {
       echo 'test successful'
      }
    }

    stage('Archive') {
      steps {
        archiveArtifacts(allowEmptyArchive: true, artifacts: '*.jar')
      }
    }

  }
}