pipeline {
  agent {
    docker {
      image 'cypress/base:10'
    }
  }

  stages {
    stage('pull') {
      steps {
        git credentialsId: '5c710199-9c7f-4288-87b1-baf778e222b3', url: 'https://github.com/Tavessem/cypa.git'
      }
    }
    stage('build') {
      steps {
        sh 'npm install'
        // sh 'export DISPLAY=:99'
        // sh 'Xvfb :99'
      //bat label: "install npm package", script: 'npm install'
      }
    }
    // stage("test"){
    //     steps{
    //         sh "npm run test-s"
    //     }
    // }
    // this stage runs end-to-end tests, and each agent uses the workspace
    // from the previous stage
    stage('cypress parallel tests') {
      environment {
        // we will be recording test results and video on Cypress dashboard
        // to record we need to set an environment variable
        // we can load the record key variable from credentials store
        // see https://jenkins.io/doc/book/using/using-credentials/
        //CYPRESS_RECORD_KEY = credentials('cypress-example-kitchensink-record-key')
        // because parallel steps share the workspace they might race to delete
        // screenshots and videos folders. Tell Cypress not to delete these folders
        CYPRESS_trashAssetsBeforeRuns = 'false'
      }

      // https://jenkins.io/doc/book/pipeline/syntax/#parallel
      parallel {
        // start several test jobs in parallel, and they all
        // will use Cypress Dashboard to load balance any found spec files
        stage('tester A') {
          steps {
            echo "Running build ${env.BUILD_ID}"
            sh 'npm run test-p'
          }
        }

        // second tester runs the same command
        stage('tester B') {
          steps {
            echo "Running build ${env.BUILD_ID}"
            sh 'npm run test-p'
          }
        }

        stage('tester C') {
          steps {
            echo "Running build ${env.BUILD_ID}"
            sh 'npm run test-p'
          }
        }
      }
    }
  }
  // post {
  //   // shutdown the server running in the background
  //   always {
  //     echo 'Stopping Xvfb server'
  //     sh 'pkill'
  //   }
  // }
}
