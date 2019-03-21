pipeline {

  /*
   * Run everything on an existing agent configured with a label 'docker'.
   * This agent will need docker, git and a jdk installed at a minimum.
   */
  agent any

  // using the Timestamper plugin we can add timestamps to the console log
  options {
    timestamps()
  }


  stages {
    stage('Build') {
      steps {
        echo 'we are building here'
      }
    }

    stage('Quality Analysis') {
      parallel {
        // run Sonar Scan and Integration tests in parallel. This syntax requires Declarative Pipeline 1.2 or higher
        stage ('Integration Test') {
          agent any  //run this stage on any available agent
          steps {
            echo 'Run integration tests here...'
          }
        }
        stage('Sonar Scan') {
          steps {
            echo 'mvn sonar:sonar -Dsonar.login=$SONAR_PSW'
          }
        }
      }
    }

    stage('Build and Publish Image') {
      when {
        branch 'master'  //only run these steps on the master branch
      }
      steps {
        /*
         * Multiline strings can be used for larger scripts. It is also possible to put scripts in your shared library
         * and load them with 'libaryResource'
         */
        echo 'Here we would push to test libaries'
      }
    }
  }

  post {
    failure {
      // notify users when the Pipeline fails
      echo 'We failed, we should email the team'
    }
  }
}
