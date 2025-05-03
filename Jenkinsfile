podTemplate(
  serviceAccount: 'jenkins-admin',
  containers: [
    containerTemplate(
      name: 'jnlp',
      image: 'msd117/jenkins-generic-agent:0.0.1',
      ttyEnabled: true
    )
  ],
  namespace: 'devops-tools'
) {
  node(POD_LABEL) {
    stage('Clone') {
      container('jnlp') {
        git branch: 'main',
          credentialsId: 'github-home-kops-token',
          url: 'https://github.com/home-kops/k8s-monitoring.git'
      }
    }

    stage('Verify') {
      container('jnlp') {
        withCredentials(
          [
            string(credentialsId: 'server1-domain', variable: 'DOMAIN'),
            string(credentialsId: 'nfs1-server', variable: 'NFS_SERVER')
          ]
        ) {
          // Run the deploy script dry-run mode to validate the resources
          sh './tooling/deploy -d'
        }
      }
    }

    stage('Deploy') {
      container('jnlp') {
        if (env.BRANCH_NAME == 'main') {
          withCredentials(
            [
              string(credentialsId: 'server1-domain', variable: 'DOMAIN'),
              string(credentialsId: 'nfs1-server', variable: 'NFS_SERVER')
            ]
          ) {
            sh './tooling/deploy'
          }
        } else {
          echo "Skipping deployment as the branch is not 'main'."
        }
      }
    }
  }
}
