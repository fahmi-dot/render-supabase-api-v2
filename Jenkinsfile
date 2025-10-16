pipeline {
  agent any
  environment {
    GHCR_TOKEN = credentials('GHCR_TOKEN')
    RENDER_HOOK = credentials('RENDER_HOOK')
  }
  stages {
    stage('Checkout') {
      steps { checkout scm }
    }
    stage('Build Maven') {
      steps { sh './mvnw clean package -DskipTests' }
    }
    stage('Docker Build & Push') {
      steps {
        sh 'docker build -t ghcr.io/fahmi-dot/render-supabase-api-v2:latest .'
        sh 'echo $GHCR_TOKEN | docker login ghcr.io -u fahmi-dot --password-stdin'
        sh 'docker push ghcr.io/fahmi-dot/render-supabase-api-v2:latest'
      }
    }
    stage('Trigger Render Deploy') {
      steps {
        sh 'curl -X POST $RENDER_HOOK'
      }
    }
  }
}