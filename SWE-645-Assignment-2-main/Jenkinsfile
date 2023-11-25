pipeline {
   environment {
        registry = "adi0222/studentSurveyForm"
        registryCredential = 'DockerHubCredentials'
        TIMESTAMP = new Date().format("yyyy-MM-dd_HH-mm-ss")
        KUBECONFIG_CREDENTIALS_ID = 'K8scluster'
        DOCKER_IMAGE = "adi0222/studentsurveyform:${env.TIMESTAMP}"
    }
   agent any

   stages {
      stage('Build') {
         steps {
            script{
               sh 'rm -rf ../*.war'
               dir('src/main/webapp') {
                        // Specify the path for the war file creation
                        sh 'jar -cvf ../surveyform.war .'
               }
               sh 'echo ${BUILD_TIMESTAMP}'
               docker.withRegistry('',registryCredential){
                  def customImage = docker.build("$DOCKER_IMAGE")
               }
            }
         }
      }

      stage('Push Image to Dockerhub') {
         steps {
            script{
               docker.withRegistry('',registryCredential){
                  sh "docker push $DOCKER_IMAGE"
               }
            }
         }
      }

      stage('Deploying to Rancher as Load Balancer') {
            steps {
                withCredentials([file(credentialsId: "$KUBECONFIG_CREDENTIALS_ID", variable: 'KUBECONFIG')]) {
                    sh 'kubectl set image deployment/surveyformlb container-0=$DOCKER_IMAGE'
                }
            }
        }

        stage('Deploying to Rancher as Node Port') {
            steps {
                withCredentials([file(credentialsId: "$KUBECONFIG_CREDENTIALS_ID", variable: 'KUBECONFIG')]) {
                    sh 'kubectl set image deployment/surveyformnp container-0=$DOCKER_IMAGE'
                }
            }
        }
   }
}
