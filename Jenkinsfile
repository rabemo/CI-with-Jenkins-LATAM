pipeline {
 
 agent any
 
 environment {
    PROJECT_ID = "vaulted-quarter-260801"
    CLUSTER_NAME = 'kube-demo'
    LOCATION = 'us-central1-c'
    CREDENTIALS_ID = 'JSON'
  }    
 stages {
     stage('Checkout SCM') {
      steps {
       checkout scm
      }
     }
    stage('Build package') {

        steps {
        echo "Cleaning and packing.."
         sh 'mvn clean package'
        }
    }
     stage('Test') {
      steps {
       echo "Testing.."
       sh 'mvn test'
      }
     }
 
     stage('Build Docker Image') {
      steps {
       echo "Build Docker Image.."
       withCredentials([usernamePassword(credentialsId: 'docker-login-creds', passwordVariable: 'password', usernameVariable: 'username')]) {
         echo "${password} | docker login -u ${username} --password-stdin"
        }
       sh "docker build -t rabemo/devops:${env.BUILD_ID}"
      }
     }
  
  
  //   stage('Build and push Docker Image') {
  //    steps{
  //      script {
  //         appimage = docker.build( "rabemo/devops:c")
  //         docker.withRegistry("https://registry.hub.docker.com",'docker-hub-credentials') 
  //         appimage.push("${env.BUILD_ID}") 
  //         }
  //       }
  //     }
     }
    
  //  stage('Deploy to Kubernetes') {
  //    steps {
  //     echo "Deploying to Kubernetes Cluster.."
  //     sh 'ls -ltr'
  //     sh 'pwd'
  //     sh "sed -i 's/tagversion/${env.BUILD_ID}/g' deployment.yaml"
  //     step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'deployment.yaml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
  //     echo "Deployment to Kubernetes cluster completed.."
  //   }
  //  }
  //  }
}
