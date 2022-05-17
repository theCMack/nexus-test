pipeline {

  agent any

  environment {
    NPM_AUTH_TOKEN = credentials('npm-stengel')
    NPM_PUBLISH_TOKEN = credentials('npm-stengel')
    HOME = '.'
  }

  options {
    timeout(time: 60, unit: 'MINUTES')
    disableConcurrentBuilds()
  }

  stages {

    stage('build dependencies') {
      when {
        anyOf {
          branch 'master';
          branch 'release*';
        }
      }
      steps {
        // On the master branch, bump the patch version, and push
        // changes and tags out to github
        withNPM(npmrcConfig: 'stengel-npmrc') {
          sh '''
            npm install
          '''
        }
      }
    }

    stage('publish package') {
      when {
        anyOf {
          branch 'master';
          branch 'release*';
        }
      }
      steps {
        // On the master branch, bump the patch version, and push
        // changes and tags out to github
        withNPM(npmrcConfig: 'stengel-npmrc') {
          sh '''
            HUSKY_SKIP_INSTALL=1 CI=true npm publish
          '''
        }
      }
    }
  }
}
