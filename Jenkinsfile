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

    stage('use group repo') {
      when {
        not {
          anyOf {
            branch 'master';
            branch 'release*';
            expression { return env.CHANGE_ID }
          }
        }
      }
      steps {
        sh '''
        cp .npmrc_group .npmrc
        echo "registry=https://artifacts.unchained-capital.us/repository/npm-stengel-group/" >> .npmrc
        '''
      }
    }

    stage('Use trusted repo') {
      when {
        anyOf {
          branch 'master';
          branch 'release*';
          expression { return env.CHANGE_ID }
        }
      }
      steps {
          sh '''
          cp .npmrc_group .npmrc
          echo "registry=https://artifacts.unchained-capital.us/repository/npm-stengel-group/" >> .npmrc
          '''
      }
    }

    stage('build dependencies') { 
      steps {
        sh 'npm install' 
      }
    }

    stage('Publish Package') {
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
