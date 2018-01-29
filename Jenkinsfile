pipeline {
  agent {
    label "prod"
  }
  options {
    buildDiscarder(logRotator(numToKeepStr: '2'))
    disableConcurrentBuilds()
  }
  stages {
    stage("notify") {
      steps {
        slackSend(
          color: "info",
          message: "${env.JOB_NAME} started: ${env.RUN_DISPLAY_URL}"
        )
      }
    }
    stage("setup") {
      when {
        branch "master"
      }
      steps {
        sh "docker network create --driver overlay swarm-visualizer || echo 'Network creation failed. It probably already exists.'"
      }
    }
    stage("deploy") {
      when {
        branch "master"
      }
      environment {
        SWARM_VIS_DOMAIN = "${env.swarmVisualizerDomain}"
      }
      steps {
        script {
          if (env.swarmVisualizerDomain) {
            sh "docker stack deploy -c swarm-visualizer.yml swarm-visualizer"
          } else {
            sh 'echo "ERROR: env.swarmVisualizerDomain is required." && exit 1'
          }
        }
      }
    }
  }
  post {
    always {
      sh "docker system prune -f"
    }
    failure {
      slackSend(
        color: "danger",
        message: "${env.JOB_NAME} failed: ${env.RUN_DISPLAY_URL}"
      )
    }
    success {
      slackSend(
        color: "success",
        message: "${env.JOB_NAME} succeeded: ${env.RUN_DISPLAY_URL}"
      )
    }
  }
}