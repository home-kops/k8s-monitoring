pipeline {
  agent {
    kubernetes {
      label 'agent-s'
    }
  }
  stages {
    stage('Clone') {
      steps {
        git branch: 'main',
          credentialsId: 'github-home-kops-token',
          url: 'https://github.com/home-kops/k8s-monitoring.git'
      }
    }

    stage('Verify') {
      steps {
        container('jnlp') {
          sh '''
            helm lint ./alloy && \
              helm dependency build ./alloy && \
              helm template ./alloy -f ./alloy/values.yaml
          '''

          sh '''
            helm repo add crowdsec https://crowdsecurity.github.io/helm-charts && \
              helm repo update && \
              helm template crowdsec crowdsec/crowdsec -f ./crowdsec/values.yaml
          '''

          sh '''
            helm lint ./grafana && \
              helm dependency build ./grafana && \
              helm template ./grafana -f ./grafana/values.yaml
          '''

          sh '''
            helm repo add grafana https://grafana.github.io/helm-charts && \
              helm repo update && \
              helm template loki grafana/loki -f ./loki/values.yaml
          '''

          sh '''
            helm repo add prometheus-community https://prometheus-community.github.io/helm-charts && \
              helm repo update && \
              helm template prometheus prometheus-community/prometheus -f ./prometheus/values.yaml
          '''
        }
      }
    }
  }
}
