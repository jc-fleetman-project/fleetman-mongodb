pipeline {
   agent any

   environment {
     // Name des Service
     SERVICE_NAME = "fleetman-mongodb"

     // Tag für Docker, bestehend aus Repository Name und Tag des Image
     REPOSITORY_TAG ="${YOUR_DOCKERHUB_USERNAME}/${SERVICE_NAME}:${BUILD_ID}"

     // GCE
     PROJECT_ID = 'capable-arbor-268514'
     CLUSTER_NAME = 'cluster-1'
     LOCATION = 'us-central1-a'
     MANIFEST = 'deploy.yaml'
     CREDENTIALS_ID = 'gke'
   }

   stages {
      stage('Preparation') {
         steps {
            cleanWs()
            git credentialsId: 'GitHub', url: "https://github.com/${ORGANIZATION_NAME}/${SERVICE_NAME}"
         }
      }

      stage('Deploy to Cluster') {
          steps {
            // Führt das Deployment aus
            step([
              $class: 'KubernetesEngineBuilder',
              projectId: env.PROJECT_ID,
              clusterName: env.CLUSTER_NAME,
              location: env.LOCATION,
              manifestPattern: env.MANIFEST,
              credentialsId: env.CREDENTIALS_ID,
              verifyDeployments: true])
          }
      }
   }
}
