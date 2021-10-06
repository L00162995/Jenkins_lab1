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

    stage('Test') { 
      steps {
       echo 'test successful'
      }
    }

    stage('Archive and report') {
      parallel {
        stage ('archive the Artifacts') {
          steps {
          archiveArtifacts(allowEmptyArchive: true, artifacts: 'target/*.jar')
          }
        }
        stage('send an email'){
          echo 'sending email' 
///          emailext body: "Jenkins_lab1 Pipeline for ${env.BRANCH_NAME} was triggered",
///            subject: "Jenkins_lab1 Pipeline for ${BRANCH_NAME} with ${env.BUILD_STATUS}",
///            to: 'L00162995@student.lyit.ie'
        }
        
      }
      
    }
  }
}