pipeline {
   agent any

   environment {
     // You must set the following environment variables
     // ORGANIZATION_NAME
     // YOUR_DOCKERHUB_USERNAME (it doesn't matter if you don't have one)
     
     SERVICE_NAME = "docker-java-helloworld-pipeline"
     IMAGE_NAME = "ci-pipeline-demo-${jenkins_username}"
     REPOSITORY_TAG="${DOCKERHUB_URL}/${IMAGE_NAME}:${BUILD_ID}"
   }

   stages {
      stage('Preparation') {
         steps {
            git credentialsId: 'GitHub', url: "https://github.com/${ORGANIZATION_NAME}/${SERVICE_NAME}"
         }
      }
      stage('Build') {
         steps {
            sh '''mvn clean install package'''
         }
      }

      stage('SonarQube') {
         steps {
            sh '${sonarcli}'
         }
      }
      stage('Build Image') {
         steps {
           sh 'docker image build -t ${REPOSITORY_TAG} /home/jenkins/docker/${BUILD_ID}'
         }
      }
      stage('Push Image to repo') {
          steps {
           sh 'docker push ${REPOSITORY_TAG}'
          }
      }
   }
}
