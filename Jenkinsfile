pipeline {

  // WHERE to run — 'any' = any available agent/node
  agent any

  // GLOBAL ENV VARIABLES — available in all stages
  environment {
    APP_NAME  = 'my-app'
    VERSION   = '1.0.0'
  }

  // TOOLS — auto-provisioned by Global Tool Config
  tools {
    nodejs 'NodeJS-18'
  }

  // PIPELINE STAGES
  stages {

    stage('Checkout') {
      steps {
        git branch: 'main',
            url: 'https://github.com/YOUR/REPO.git'
      }
    }

    stage('Install') {
      steps {
        sh 'npm install'
      }
    }

    stage('Test') {
      steps {
        sh 'npm test'
      }
      // What to do if test stage fails
      post {
        failure { echo 'Tests failed! Fix before merging.' }
      }
    }

    stage('Build') {
      steps {
        sh 'npm run build'
      }
    }

    stage('Deploy') {
      // Only deploy from 'main' branch
      when { branch 'main' }
      steps {
        sh './scripts/deploy.sh'
      }
    }

  }

  // POST — runs after all stages complete
  post {
    always  { echo 'Pipeline finished.' }
    success { echo '✅ Build passed!' }
    failure { echo '❌ Build failed — check logs.' }
    cleanup { cleanWs() }
  }

}
