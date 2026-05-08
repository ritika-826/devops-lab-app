pipeline {

  agent any

  environment {
    APP_NAME = 'devops-lab-app'
  }

  tools {
    nodejs 'NodeJS-18'
  }

  stages {

    stage('Checkout') {
      steps {
        git branch: 'main',
            url: 'https://github.com/ritika-826/devops-lab-app.git'
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
    }

    stage('Build') {
      steps {
        sh 'npm run build'
      }
    }

    stage('Deploy') {
      steps {
        sh '''
        docker stop devops-app || true
        docker rm devops-app || true

        docker build -t devops-lab-app .

        docker run -d \
          --name devops-app \
          -p 3000:3000 \
          devops-lab-app
        '''
      }
    }

  }

  post {
    always {
      echo 'Pipeline finished.'
    }

    success {
      echo '✅ Build passed!'
    }

    failure {
      echo '❌ Build failed.'
    }
  }

}
