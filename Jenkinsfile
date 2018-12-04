pipeline {
  options {
    disableConcurrentBuilds()
  }
  agent {
    kubernetes {
        // Change the name of jenkins-maven label to be able to use yaml configuration snippet
        label "maven-dind"
        // Inherit from Jx Maven pod template
        inheritFrom "maven"
        // Add pod configuration to Jenkins builder pod template
        yamlFile "maven-dind.yaml"
      } 
  }
  environment {
    DEPLOY_NAMESPACE = "jx-staging"
    CHART_REPOSITORY = "https://almerico.github.io/helmrepo"
  }
  stages {
    stage('Validate Environment') {
      steps {
        container('maven') {
          dir('env') {
            sh 'jx step helm build'
          }
        }
      }
    }
    stage('Update Environment') {
      when {
        branch 'master'
      }
      steps {
        container('maven') {
          dir('env') {
            sh 'cat values.yaml'
            sh 'ls'
            sh 'jx step helm apply'
          }
        }
      }
    }
  }
}
